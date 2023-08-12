<script>
	// props
	export let expand = 1;
	export let type = 'normal';
	export let clicked = () => {
		console.log('I am being clicked.');
	};
</script>

<button
	type={type === 'submit' ? 'submit' : 'button'}
	class="{type} key-pad"
	style="grid-column: span {expand};"
	on:click={clicked}
>
	<span><slot>C</slot></span>
</button>

<style>
	.accent-color {
		background-color: #f8a100 !important;
		aspect-ratio: unset !important;
	}
	.key-pad {
		aspect-ratio: 1;
		background-color: white;
		border: 0px;
		padding: 22px 20px;
		margin: 0;
		font-size: 16px;
		border-radius: 0.75rem;
		position: relative;
		transition: all 0.1s ease-in;
	}
	.key-pad span {
		position: absolute;
		font-weight: 500;
		font-size: 1.2rem;
		bottom: 1rem;
		left: 1rem;
	}

	.key-pad:active {
		animation-name: example;
		animation-duration: 0.5s;
	}
	.key-pad:hover {
		transform: scale(0.975, 0.975);
	}

	@keyframes example {
		0% {
			transform: scale(1, 1);
		}
		50% {
			transform: scale(0.9, 0.9);
		}
		100% {
			transform: scale(0.975, 0.975);
		}
	}
</style>
