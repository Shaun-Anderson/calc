<script lang="ts">
	import { fly, fade } from 'svelte/transition';
	import Keypad from '../components/keypad.svelte';

	let numberInput = '';
	let total = 0;
	let history: string[] = [];

	// private function
	function addToEquation(value) {
		numberInput += value;
	}

	const clear = () => {
		total = 0;
		numberInput = '';
	};

	function calculate() {
		if (numberInput !== '') {
			console.log('CALC');
			const calc = numberInput;
			numberInput = result().toString();
			history = [...history, `${calc}=${numberInput}`];
		}
	}

	function replaceAll(string, search, replace) {
		return string.split(search).join(replace);
	}

	function formatString(value) {
		return replaceAll(replaceAll(value, '*', 'x'), '/', '÷');
	}

	// computed
	let result = () => {
		// if(isNaN(nu))
		if (!isNaN(numberInput.slice(-1))) {
			return eval(numberInput);
		}
		return eval(numberInput.slice(0, -1));
	};

	// reactive values
	// $: if (numberInput !== '' && !isNaN(numberInput.slice(-1)) && numberInput != result()) {
	// 	total = result().toString();
	// }
</script>

<main class="main">
	<div style="width: 100%; display: flex; justify-content: center;">
		<ul class="history">
			{#each history as historyEntry, index}
				<li out:fade={{ duration: 250 }} in:fly={{ x: 50 }}>{historyEntry}</li>
			{/each}
		</ul>
	</div>
	<form
		class="device"
		on:submit={(e) => {
			e.preventDefault();
			calculate();
		}}
	>
		<div class="screen">
			<!-- <div class="calculations">{numberInput}</div> -->

			<input
				type="text"
				class="total"
				value={numberInput}
				on:change={(e) => (numberInput = e.currentTarget.value)}
			/>
		</div>
		<!-- <div><button class="side-button" /></div> -->

		<div class="key-pad-container">
			<Keypad clicked={() => clear()}>C</Keypad>
			<Keypad clicked={() => addToEquation('(')}>(</Keypad>
			<Keypad clicked={() => addToEquation(')')}>)</Keypad>
			<Keypad clicked={() => addToEquation('/')}>/</Keypad>
			<Keypad clicked={() => addToEquation(1)}>1</Keypad>
			<Keypad clicked={() => addToEquation(2)}>2</Keypad>
			<Keypad clicked={() => addToEquation(3)}>3</Keypad>
			<Keypad clicked={() => addToEquation('*')}>x</Keypad>
			<Keypad clicked={() => addToEquation(4)}>4</Keypad>
			<Keypad clicked={() => addToEquation(5)}>5</Keypad>
			<Keypad clicked={() => addToEquation(6)}>6</Keypad>
			<Keypad type="operator" clicked={() => addToEquation('-')}>-</Keypad>
			<Keypad clicked={() => addToEquation(7)}>7</Keypad>
			<Keypad clicked={() => addToEquation(8)}>8</Keypad>
			<Keypad clicked={() => addToEquation(9)}>9</Keypad>
			<Keypad clicked={() => addToEquation('+')}>+</Keypad>
			<Keypad clicked={() => addToEquation(0)}>0</Keypad>
			<Keypad clicked={() => addToEquation('.')}>.</Keypad>
			<Keypad expand={2} type="accent-color" clicked={() => calculate()}>=</Keypad>
		</div>
	</form>
</main>
<footer>Crafted by Shaun Anderson</footer>

<style>
	footer {
		color: gray;
		font-weight: 100;
		opacity: 0.8;
		position: absolute;
		bottom: 1rem;
		width: 100%;
		text-align: center;
		z-index: -1;
	}
	.history {
		display: flex;
		justify-content: end;
		flex-direction: column;
		align-items: end;
		padding: 0;
		flex-shrink: 1;

		/* border: 1px solid gray; */
		text-align: right;
		width: 100%;
		max-width: 300px;
		/* min-height: 100px; */
		height: 200px;
		max-height: 200px;
		overflow: auto;
		font-size: 0.8rem;
	}
	.history li {
		list-style: none;
		transition: opacity 1s ease-in-out;
	}
	.history:not(:hover) li:nth-last-child(-n + 3) {
		opacity: 1 !important;
	}
	.history:not(:hover) li:nth-last-child(4) {
		opacity: 0.5 !important;
	}
	.history:not(:hover) li:nth-last-child(5) {
		opacity: 0.33 !important;
	}
	.history:not(:hover) li:nth-last-child(6) {
		opacity: 0.25 !important;
	}
	.history:not(:hover) li {
		opacity: 0.1;
	}

	.side-button {
		border-radius: 0px 5px 5px 0px;
		background-color: #f8a100 !important;
		border: 0;
		position: absolute;
		right: -0rem;
		height: 50px;
	}
	.calculations {
		color: aliceblue;
	}
	.main {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100vh;
	}
	.device {
		position: relative;
		/* padding: 1rem; */
		flex-grow: 1;
		flex-shrink: 0;
		max-width: 400px;
		width: 100%;
		border-radius: 1rem;
		border: 1px solid #ffff;
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}
	.screen {
		display: flex;
	}
	.screen input {
		width: 100%;
		font-size: 1.25rem;
		text-align: right;
		background-color: black;
		padding: 1rem;
		border-radius: 0.75rem;
		color: white;
		/* background-color: transparent; */
		border: 0px;
	}
	.key-pad-container {
		padding: 0.25rem;
		border-radius: 1rem;
		display: grid;
		grid-template-columns: 1fr 1fr 1fr 1fr;
		background-color: black;
	}
</style>
