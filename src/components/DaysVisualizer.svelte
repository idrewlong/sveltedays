<script lang="ts">
	import { onMount } from 'svelte';

	let selectedYear = Math.max(2020, new Date().getFullYear());
	let currentDate = new Date();

	function isLeapYear(year: number): boolean {
		return year % 400 === 0 || (year % 100 !== 0 && year % 4 === 0);
	}

	function calculateDayOfYear(date: Date): number {
		const start = new Date(date.getFullYear(), 0, 0);
		const diff = date.getTime() - start.getTime();
		const oneDay = 1000 * 60 * 60 * 24;
		return Math.floor(diff / oneDay);
	}

	// Adjust columns for mobile
	let COLUMNS = 25;

	// Reactive columns based on window width
	$: if (typeof window !== 'undefined') {
		COLUMNS = window.innerWidth < 640 ? 15 : 25;
	}

	// Create yearRange first
	const yearRange = Array.from({ length: 100 }, (_, i) => new Date().getFullYear() - 25 + i);

	// Then use reactive statements
	$: currentYear = currentDate.getFullYear();
	$: totalDays = 365 + (isLeapYear(selectedYear) ? 1 : 0);
	$: currentDayOfYear = calculateDayOfYear(currentDate);
	$: rows = Math.ceil(totalDays / COLUMNS);
	$: days = Array.from({ length: totalDays }, (_, i) => ({
		day: i + 1,
		isPast: selectedYear < currentYear || (selectedYear === currentYear && i < currentDayOfYear)
	}));
	$: remainingDays =
		selectedYear < currentYear
			? 0
			: selectedYear === currentYear
				? totalDays - currentDayOfYear
				: totalDays;
	$: percentageLeft =
		selectedYear < currentYear ? 0 : Math.round((remainingDays / totalDays) * 100);

	function getDayDate(dayNumber: number): string {
		const date = new Date(selectedYear, 0);
		date.setDate(dayNumber);
		return date.toLocaleDateString('en-US', {
			month: 'long',
			day: 'numeric',
			year: 'numeric'
		});
	}

	// Group years by decade for better organization
	const decades = Array.from({ length: 10 }, (_, i) => {
		const decadeStart = Math.floor(new Date().getFullYear() / 10) * 10 - 40 + i * 10;
		return {
			label: `${decadeStart} - ${decadeStart + 9}`,
			years: Array.from({ length: 10 }, (_, j) => decadeStart + j)
		};
	});
</script>

<div class="flex min-h-screen w-full flex-col bg-black px-6 md:py-8">
	<div
		class="mb-4 flex flex-none flex-col gap-4 sm:mb-6 sm:flex-row sm:items-center sm:justify-between"
	>
		<h2 class="text-xl font-semibold text-white sm:text-2xl">Days Left in {selectedYear}</h2>
		<div class="relative w-full sm:w-48">
			<select
				bind:value={selectedYear}
				class="w-full appearance-none rounded-lg bg-gray-800 px-4 py-2.5 text-base text-white transition-all
					   hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-white/20 sm:text-lg"
			>
				<optgroup label="Current Decade">
					{#each Array.from({ length: 10 }, (_, i) => {
						const year = Math.max(2020, Math.floor(currentYear / 10) * 10 + i);
						return year;
					}) as year}
						<option value={year} selected={year === currentYear}>{year}</option>
					{/each}
				</optgroup>
				<optgroup label="Future">
					{#each Array.from({ length: 50 }, (_, i) => currentYear + (i + 1)) as year}
						<option value={year}>{year}</option>
					{/each}
				</optgroup>
			</select>
			<div class="pointer-events-none absolute right-2 top-1/2 -translate-y-1/2">
				<svg class="h-5 w-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M19 9l-7 7-7-7"
					/>
				</svg>
			</div>
		</div>
	</div>

	<div
		class="grid flex-1 place-items-center gap-[2px] overflow-hidden sm:gap-[3px]"
		style="grid-template-columns: repeat({COLUMNS}, minmax(0, 1fr)); grid-template-rows: repeat({rows}, minmax(0, 1fr))"
	>
		{#each days as { day, isPast }, i}
			<div
				class="h-[4px] w-[4px] rounded-full transition-opacity duration-200 {isPast
					? 'bg-gray-600'
					: 'glow-dot bg-white'}"
				title={getDayDate(day)}
			></div>
		{/each}
	</div>

	<div class="mt-4 flex justify-between text-sm text-gray-500 sm:mt-8">
		<span>{percentageLeft}% of the year left</span>
	</div>
</div>

<style>
	.grid {
		contain: strict;
	}

	.glow-dot {
		box-shadow: 0 0 4px rgba(255, 255, 255, 0.4);
	}

	@media (min-width: 640px) {
		div[title] {
			height: 10px;
			width: 10px;
		}

		.glow-dot {
			box-shadow: 0 0 6px rgba(255, 255, 255, 0.5);
		}
	}
</style>
