<script>
	import Slider from './lib/Slider.svelte';
	import Checkbox from './lib/Checkbox.svelte';

	let pwGenerated = false
	let charLength = 0;
	let uppercase = false;
	let lowercase = false;
	let numbers = false;
	let symbols = false;

</script>

<main>
	<h1>Password Generator</h1>
	
	<div class="password-output">
		<span class:pwGenerated class="password">P4S5W0rD!</span>
		<svg class="copy-icon" width="21" height="24" xmlns="http://www.w3.org/2000/svg"><path d="M20.341 3.091 17.909.659A2.25 2.25 0 0 0 16.319 0H8.25A2.25 2.25 0 0 0 6 2.25V4.5H2.25A2.25 2.25 0 0 0 0 6.75v15A2.25 2.25 0 0 0 2.25 24h10.5A2.25 2.25 0 0 0 15 21.75V19.5h3.75A2.25 2.25 0 0 0 21 17.25V4.682a2.25 2.25 0 0 0-.659-1.591ZM12.469 21.75H2.53a.281.281 0 0 1-.281-.281V7.03a.281.281 0 0 1 .281-.281H6v10.5a2.25 2.25 0 0 0 2.25 2.25h4.5v1.969a.282.282 0 0 1-.281.281Zm6-4.5H8.53a.281.281 0 0 1-.281-.281V2.53a.281.281 0 0 1 .281-.281H13.5v4.125c0 .621.504 1.125 1.125 1.125h4.125v9.469a.282.282 0 0 1-.281.281Zm.281-12h-3v-3h.451c.075 0 .147.03.2.082L18.667 4.6a.283.283 0 0 1 .082.199v.451Z" fill="currentColor"/></svg>
	</div>

	<div class="password-controls">
		<div class="char-len-container">
			<span class="text">Character Length</span>
			<span class="char-len">{charLength}</span>
		</div>
		
		<Slider max=20 bind:value={charLength} />

		<div class="checkbox-container">
			<Checkbox bind:checked={uppercase} label="Include Uppercase Letters" />
			<Checkbox bind:checked={lowercase} label="Include Lowercase Letters" />
			<Checkbox bind:checked={numbers} label="Include Numbers" />
			<Checkbox bind:checked={symbols} label="Include Symbols" />
		</div>
	</div>


</main>

<style>
	main {
		width: 540px;
	}

	h1 {
		font-size: 24px;
		text-align: center;
		margin: 133px 0 31px 0;
		color: var(--grey);
	}

	.password-output {
		height: 80px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		background: var(--dark-grey);
		padding: 0 32px;
		margin-bottom: 24px;
	}

	.password {
		font-size: 32px;
		color: var(--almost-white);
		opacity: 0.25;
	}

	.password.pwGenerated {
		opacity: 1;	
	}

	.copy-icon {
		cursor: pointer;
		color: var(--neon-green);
	}

	.copy-icon:hover {
		color: #FFF;
	}

	.password-controls {
		background: var(--dark-grey);
		padding: 24px 32px 32px 32px;
	}

	.char-len-container {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 18px;
	}

	.text {
		font-size: 18px;
		color: var(--almost-white);
	}

	.char-len {
		font-size: 32px;
		color: var(--neon-green);
	}

	.checkbox-container {
		margin-top: 32px;
		display: flex;
		flex-direction: column;
		gap: 19px;
		margin-bottom: 31px;
	}

</style>
