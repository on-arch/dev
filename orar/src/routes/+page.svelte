<script lang="ts">
	import * as Card from '$lib/components/ui/card';
	import { Label } from '$lib/components/ui/label';
	import { Button } from '$lib/components/ui/button'; // Assuming you have a Button component

	import {
		Omega,
		Earth,
		Angry,
		Sigma,
		PersonStanding,
		Landmark,
		Atom,
		Cpu,
		Microchip,
		Languages,
		BookMarked,
		CodeXml
	} from 'lucide-svelte';

	let currentSchedule: any;
	let simulatedDate: Date = new Date();

	const schedule = {
		primary: [
			['Electrotehnica', 'Ed. fizica', 'E.P.S.', 'Fizica', null],
			[null, 'Matematica', 'Dispozitive electronice', 'Tehnici CAD', 'L. straina'],
			['L. lit. romana', 'Fizica', 'Informatica', null, null],
			[null, null, 'Tehnici CAD', 'Electrotehnica', null],
			['L. lit. romana', 'Dispozitive electronice', 'Matematica', null, null]
		],
		secondary: [
			['Electrotehnica', 'Ed. fizica', 'Geografia', 'Matematica'],
			[null, 'Matematica', 'Dispozitive electronice', 'Tehnici CAD', 'L. straina'],
			['L. lit. romana', 'Fizica', 'Informatica', null, null],
			[null, 'Istoria', 'Tehnici CAD', 'Electrotehnica', null],
			['L. lit. romana', 'Dispozitive electronice', 'Matematica', null, null]
		]
	};

	let currentWeekendType: 'primary' | 'secondary' | null = 'primary';
	let target: any;

	const subjectIcons = {
		Electrotehnica: Omega,
		'Ed. fizica': PersonStanding,
		'E.P.S.': BookMarked,
		Fizica: Atom,
		Matematica: Sigma,
		'Dispozitive electronice': Microchip,
		'Tehnici CAD': Cpu,
		'L. straina': Languages,
		'L. lit. romana': Earth,
		Informatica: CodeXml,
		Geografia: Landmark,
		Istoria: Angry
	};

	function getDay() {
		return simulatedDate.getDay();
	}

	function determineCurrentWeekendType() {
		const day = getDay();
		if (day !== 6 && day !== 0) {
			const weekNumber = Math.floor(simulatedDate.getTime() / (1000 * 60 * 60 * 24 * 7)) % 2;
			currentWeekendType = weekNumber === 0 ? 'primary' : 'secondary';
		} else {
			currentWeekendType = null;
		}
	}

	function createSchedule() {
		determineCurrentWeekendType();
		if (currentWeekendType) {
			currentSchedule = schedule[currentWeekendType][getDay() - 1];
			console.log(target);
			if (target) target.textContent = simulatedDate.toLocaleDateString();
		}
	}

	function changeDate(days: number) {
		simulatedDate.setDate(simulatedDate.getDate() + days);
		createSchedule();
	}

	createSchedule();
</script>

<div class="grid h-screen place-items-center">
	<Card.Root>
		<Card.Header>
			<Card.Title>Orar</Card.Title>
			<Card.Description>Grupa: C-213</Card.Description>
		</Card.Header>
		<Card.Content class="flex flex-col gap-1 items-start">
			<Label
				>Data simulată: <span class="text-primary" bind:this={target}
					>{simulatedDate.toLocaleDateString()}</span
				></Label
			>
			{#if currentSchedule}
				{#each currentSchedule as clasa, i}
					<Label class="text-lg flex items-center gap-1">
						<span>{i + 1}</span>.
						{#if clasa}
							<svelte:component this={subjectIcons[clasa]} class="w-4 h-4 mr-1 text-primary" />
						{/if}
						{clasa ?? '-'}
					</Label>
				{/each}
			{/if}
		</Card.Content>
		<Card.Footer class="flex flex-col gap-2 items-start">
			<Label>Este a <span class="text-primary">{getDay()}-a</span> zi a saptaminii,</Label>
			<Label>
				fiind o saptamana <span class="text-primary">
					{currentWeekendType === 'primary'
						? 'para'
						: currentWeekendType === null
							? 'neexistenta'
							: 'impara'}
				</span>
			</Label>
			<div class="flex gap-2">
				<Button on:click={() => changeDate(-1)}>Înapoi</Button>
				<Button on:click={() => changeDate(1)}>Înafară</Button>
			</div>
		</Card.Footer>
	</Card.Root>
</div>
