<template>
	<div class="home">
		<div class="green card">
			<div class="green__header">
				<header class="green__title">
					<p>CURRENCY EXCHANGER 2000</p>	
				</header>

				<div class="green__menu">
					<!-- <button> -->
					<img src="images/signal.svg" alt="">
					<!-- </button> -->
				</div>
			</div>

			<div class="green__chip">
				<img src="images/chip.svg" alt="">
			</div>

			<div class="green__input-field">
				<input selected type="text" name="amount to convert" id="input" v-model="amountToConvert" @click="selectText" @keyup.enter="convertNow">
			</div>

			<div class="green__lower">
				<div class="green__lower-text">
					<p>input</p>
				</div>
				
				<div class="green__lower-whitespace">
					<select name="input currency" id="" v-model="convertFrom">
						<option v-for="currencyName, shorthand in availableCurrencies" :value="shorthand" :key="shorthand">
							{{ currencyName }}
						</option>
					</select>
				</div>
			</div>
		</div>

		<div class="red card">
			<div class="red__header">
				<div class="red__header-text">
					<!-- <p>input: {{convertFrom}}</p>
					<p>output: {{convertTo}}</p> -->
				</div>
			</div>

			<div class="red__output-field">
					<p>{{convertedAmount}}</p>
			</div>

			<div class="red__faketext">
				<img src="images/backwards.svg" alt="">
			</div>

			<div class="red__lower">
				<div class="red__lower-whitespace">
					<select name="output currency" id="" v-model="convertTo">
						<option v-for="currencyName, shorthand  in availableCurrencies" :value="shorthand" :key="shorthand">
							{{ currencyName }}
						</option>
					</select>
				</div>

				<div class="red__lower-text">
					<p>output</p>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			amountToConvert: 'amount + enter',
			convertedAmount: 0,
			// presets to avoid...problems
			convertFrom: 'nok',
			convertTo: 'eur',
			availableCurrencies: {},
		}
	},
	// load all available currencies from API
	async created() {
		const url = 'https://cdn.jsdelivr.net/gh/fawazahmed0/currency-api@1/latest/currencies.json'
		const response = await fetch(url)
		const result = await response.json()
		this.availableCurrencies = result;
		console.log(this.availableCurrencies)
	},
	methods: {
		// convert currencies: input * output
		async convertNow() {
			// set up some variables first
			const inputCurrency = this.convertFrom;
			const outputCurrency = this.convertTo;
			const enteredAmount = this.amountToConvert;
			// fetch the correct input currency from API
			const getCurrency = `https://cdn.jsdelivr.net/gh/fawazahmed0/currency-api@1/latest/currencies/${inputCurrency}.json`
			const response = await fetch(getCurrency);
			const result = await response.json();
			// multiply numbers with current rate of exchange, and set to data()
			let outputAmount = enteredAmount * result[inputCurrency][outputCurrency];
			console.log(outputAmount)
			if (Number.isNaN(outputAmount)) {
				return this.convertedAmount = 'invalid input';
			} else {
				return this.convertedAmount = outputAmount;
				}	
		}
 	},
	computed: {
		// select all text at click on input-field
		selectText() {
			const input = document.getElementById('input');
  			input.focus();
  			input.select();
		}
	}
}
</script>

<style>
	.home {
		color: white;
		margin: 0 auto;
		height: 95vh;
		min-height: 540px;
		width: 56vh;
		min-width: 302px;
		border: thin solid black;
		border-radius: 15px;
	}
	.card {
		margin: 20% auto;
		height: 31%;
		width: 90%;
		border-radius: 15px;
	}
	.green {
		padding: 5%;
		background: #2D888D;
	}

	.green__header {
		display: flex;
		flex-flow: row nowrap;
		align-items: flex-start;
		justify-content: space-between;
	}
	.green__chip img {
		margin: 4% 0;
		width: 19%;
	}

	.green input {
		color: white;
		width: 90%;
		font-size: 1.2rem;
		border-bottom: thin solid transparent;
		animation: highlightField 3s infinite;
	}

	.green__lower, .red__lower {
		font-size: 0.8rem;
		display: flex;
		flex-flow: row nowrap;
		align-items: flex-end;
		justify-content: space-between;
	}

	.green__lower select {
		position: absolute;
		top: 19%;
		border: none;
		width: 100%;
	}

	.green__lower-whitespace, .red__lower-whitespace {
		width: 50%;
		height: 1.8rem;
		margin-top: 4%;
		background: white;
		color: black;
		border-radius: 5px;
		position: relative;
	}

	.red {
		background: #A3393B;
		padding-top: 5%;
	}
	.red__header {
		width: 100%;
		height: 2rem;
		background: black;
	}

	.red__output-field {
		width: 90%;
		margin: 5.5% auto;
		height: 2rem;
		background: white;
		color: black;
		font-size: 1.2rem;
		border-radius: 5px;
	}
	.red__output-field p {
		padding: 2% 0 0 2%;
	}
	.red__faketext img {
		width: 75%;
		margin-left: 14%;
	}
	.red__lower-whitespace {
		margin-left: 5%;
	}
	.red__lower-text {
		margin-right: 5%;
	}
	.red__lower select {
		position: absolute;
		top: 19%;
		border: none;
		width: 100%;
	}

	@keyframes highlightField {
		0% {border-bottom: thin solid transparent;}
		50% {border-bottom: thin solid white;}
		100% {border-bottom: thin solid transparent;}
	}
</style>