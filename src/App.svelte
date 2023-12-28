<script>
	import { fade } from 'svelte/transition';
	import Slider from './lib/Slider.svelte';
	import Checkbox from './lib/Checkbox.svelte';

	let password = 'P4S5W0rD!';
	let canCopy = false;
	let copied = false;
	let length = 0;
	let strengthDescription = '';
	let strengthBars;

	const charSets = {
		uppercase: ['ABCDEFGHIJKLMNOPQRSTUVWXYZ', false],
		lowercase: ['abcdefghijklmnopqrstuvwxyz', false],
		numbers: ['1234567890', false],
		symbols: ['!@#$%^&*()', false]
	};

	function generatePassword() {
		let includedChars = '';
		let charPool = 0;
		let newPassword = '';
		copied = false;

		Object.keys(charSets).forEach(set => {
			if (charSets[set][1]) {
				includedChars += charSets[set][0];
			}
		});

		if (length === 0 || includedChars === '') {
			password = 'P4S5W0rD!';
			strengthDescription = '';
			resetBars();
			canCopy = false;
			return;
		}

		charPool = includedChars.length;

		for (let i = 0; i < length; i++) {
			newPassword += includedChars.charAt(Math.floor(Math.random() * charPool));
		}

		let passwordStrength = calcStrength(length, charPool);
		
		styleBars(passwordStrength[1]);
		strengthDescription = String(passwordStrength[0]);
		password = newPassword;
		canCopy = true;
	}

	function calcStrength(length, charPool) {
		// calculate password entropy: https://www.omnicalculator.com/other/password-entropy
		let strength = length * Math.log2(charPool);
		
		if (strength < 25) { return ['Too Weak!', 1] }
		else if (strength < 50) { return ['Weak', 2] }
		else if (strength < 75) { return ['Medium', 3] }
		else { return ['Strong', 4] };
	}

	function styleBars(numBars) {
		resetBars();
		
		let barColor;
		switch (numBars) {
			case 1:
				barColor = 'var(--red)';
				break;
			case 2:
				barColor = 'var(--orange)';
				break;
			case 3:
				barColor = 'var(--yellow)';
				break;
			case 4:
				barColor = 'var(--neon-green)';
		};

		for (let i = 0; i < numBars; i++) {
			strengthBars.children[i].style.backgroundColor = barColor;
			strengthBars.children[i].style.borderColor = barColor;
		}
	}

	function resetBars() {
		Array.from(strengthBars.children).forEach(bar => {
			bar.style.backgroundColor = 'transparent';
			bar.style.borderColor = 'var(--almost-white)';
		});
	}

	async function copyPassword() {
		if (!canCopy) return;
		await navigator.clipboard.writeText(password);
		copied = true;

		// remove copied notification after 2 seconds
		setTimeout(() => {
			copied = false;
		}, 2000);
	}

</script>

<main>
	<h1>Password Generator</h1>
	
	<div class="password-output">
		<span class:canCopy class="password">{password}</span>
		<div class="copy-container">
			{#if copied}
				<span out:fade={{duration: 500}} class="copy-confirm">Copied</span>
			{/if}
			<button type="button" on:click={copyPassword}>
				<svg class="copy-icon" width="21" height="24" xmlns="http://www.w3.org/2000/svg"><path d="M20.341 3.091 17.909.659A2.25 2.25 0 0 0 16.319 0H8.25A2.25 2.25 0 0 0 6 2.25V4.5H2.25A2.25 2.25 0 0 0 0 6.75v15A2.25 2.25 0 0 0 2.25 24h10.5A2.25 2.25 0 0 0 15 21.75V19.5h3.75A2.25 2.25 0 0 0 21 17.25V4.682a2.25 2.25 0 0 0-.659-1.591ZM12.469 21.75H2.53a.281.281 0 0 1-.281-.281V7.03a.281.281 0 0 1 .281-.281H6v10.5a2.25 2.25 0 0 0 2.25 2.25h4.5v1.969a.282.282 0 0 1-.281.281Zm6-4.5H8.53a.281.281 0 0 1-.281-.281V2.53a.281.281 0 0 1 .281-.281H13.5v4.125c0 .621.504 1.125 1.125 1.125h4.125v9.469a.282.282 0 0 1-.281.281Zm.281-12h-3v-3h.451c.075 0 .147.03.2.082L18.667 4.6a.283.283 0 0 1 .082.199v.451Z" fill="currentColor"/></svg>
			</button>
		</div>
	</div>

	<div class="password-controls">
		<div class="char-len-container">
			<span class="text">Character Length</span>
			<span class="char-len">{length}</span>
		</div>
		
		<Slider max=20 bind:value={length} />

		<div class="checkbox-container">
			<Checkbox bind:checked={charSets.uppercase[1]} label="Include Uppercase Letters" />
			<Checkbox bind:checked={charSets.lowercase[1]} label="Include Lowercase Letters" />
			<Checkbox bind:checked={charSets.numbers[1]} label="Include Numbers" />
			<Checkbox bind:checked={charSets.symbols[1]} label="Include Symbols" />
		</div>

		<div class="strength-container">
			<span class="strength-label">Strength</span>
			<span class="strength-rating">{strengthDescription}</span>
			<div class="strength-bars" bind:this={strengthBars}>
				<div class="bar 1"></div>
				<div class="bar 2"></div>
				<div class="bar 3"></div>
				<div class="bar 4"></div>
			</div>
		</div>

		<button class="generate-pw-btn" on:click={generatePassword} type="button">
			Generate
			<svg width="12" height="12" xmlns="http://www.w3.org/2000/svg"><path fill="currentColor" d="m5.106 12 6-6-6-6-1.265 1.265 3.841 3.84H.001v1.79h7.681l-3.841 3.84z"/></svg>
		</button>
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

	button { 
		cursor: pointer;
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

	.password.canCopy {
		opacity: 1;	
	}

	.copy-container {
		display: flex;
		align-items: center;
		position: relative;
	}
	
	.copy-confirm {
		font-size: 18px;
		line-height: 32px;
		color: var(--neon-green);
		text-transform: uppercase;
		margin-right: 16px;
		position: absolute;
		left: -80px;
		padding: 0 10px;
		background: var(--dark-grey);
	}

	.copy-icon {
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

	.strength-container {
		height: 72px;
		background: var(--very-dark-grey);
		padding: 0 32px; 
		display: flex;
		align-items: center;
		margin-bottom: 32px;
	}

	.strength-label {
		font-size: 18px;
		text-transform: uppercase;
		color: var(--grey);
	}

	.strength-rating {
		font-size: 24px;
		text-transform: uppercase;
		color: var(--almost-white);
		margin-left: auto;
		margin-right: 15.5px;
	}

	.strength-bars{
		height: 28px;
		display: flex;
		justify-content: space-between;
		gap: 8px;
	}

	.bar {
		width: 10px;
		border: 2px solid var(--almost-white);
	}

	.generate-pw-btn {
		height: 65px;
		background: var(--neon-green);
		width: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 24px;
		font-size: 18px;
		color: var(--dark-grey);
		text-transform: uppercase;
		padding-top: 2px;
	}

	.generate-pw-btn > svg {
		color: var(--dark-grey);
	}

	.generate-pw-btn:hover {
		background: var(--dark-grey);
		border: 2px solid var(--neon-green);
		color: var(--neon-green);
	}

	.generate-pw-btn:hover > svg {
		color: var(--neon-green);
	}

	@media (max-width: 580px) {
		main {
			width: 343px;
		}

		h1 {
			font-size: 16px;
		}

		.password-output {
			height: 64px;
			padding: 0 16px;
			margin-bottom: 16px;
		}

		.password {
			font-size: 24px;
		}

		.copy-confirm {
			font-size: 16px;
		}

		.password-controls {
			padding: 21px 16px 16px 16px;
		}

		.char-len-container {
			margin-bottom: 18px;
		}

		.text {
			font-size: 16px;
		}

		.char-len {
			font-size: 24px;
		}

		.checkbox-container {
			margin-top: 32px;
			gap: 16px;
			margin-bottom: 31px;
		}

		.strength-container {
			height: 56px;
			padding: 0 16px; 
			margin-bottom: 16px;
		}

		.strength-label {
			font-size: 16px;
		}

		.strength-rating {
			font-size: 18px;
			margin-right: 16px;
		}

		.generate-pw-btn {
			height: 56px;
			width: 100%;
			gap: 16px;
			font-size: 16px;
			padding-top: 2px;
		}
	}

</style>
