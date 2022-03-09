<template>
	<div class="home">
		<div class="green card">
			<div class="green__header">
				<header class="green__title">
					<p>CURRENCY EXCHANGER 2000</p>	
				</header>

				<div class="green__menu">
					<button>
						<img src="images/signal.svg" alt="">
					</button>
				</div>
			</div>

			<div class="green__chip">
				<img src="images/chip.svg" alt="">
			</div>

			<div class="green__input-field">
				<input type="text" name="amount to convert" id="input" v-model="amountToConvert" @click="selectText" @keyup.enter="convertNow">
			</div>

			<div class="green__lower">
				<div class="green__lower-text">
					<p>input</p>
				</div>
				<div class="green__lower-whitespace">
					<!-- <input type="" class="green__lower-currency"> -->
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
			
					<!-- <input type="" class="green__lower-currency"> -->
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
			// add numbers with current rate of exchange, and set to data()
			// tried and failed to use failsafe :-(
			let outputAmount = enteredAmount * result[inputCurrency][outputCurrency];
			console.log(outputAmount)
			if (outputAmount === NaN) {
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
		height: 568px;
		width: 320px;
		border: thin solid black;
		border-radius: 15px;
	}
	.card {
		margin: 20% auto;
		width: 290px;
		height: 175px;
		border-radius: 15px;
	}
	.green {
		padding: 1rem;
		background: #2D888D;
	}
	.green__header {
		display: flex;
		flex-flow: row nowrap;
		align-items: flex-start;
		justify-content: space-between;
	}
	.green__chip img {
		margin: 10px 0 10px 0;
		height: 38px; 
		width: 46px;
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
		top: 7px;
		border: none;
		width: 100%;
	}

	.green__lower-whitespace, .red__lower-whitespace {
		width: 50%;
		height: 28px;
		margin-top: 10px;
		background: white;
		color: black;
		border-radius: 5px;
		position: relative;
	}

	.red {
		background: #A3393B;
		padding-top: 1rem;
	}
	.red__header {
		/* margin-top: 1rem; */
		width: 100%;
		height: 2rem;
		background: black;
	}
	.red__header-text {
		display: flex;
		flex-flow: row nowrap;
		justify-content: space-between;
		color: white;
		padding: 7px 1rem 0 1rem;
	}
	.red__output-field {
		width: 90%;
		margin: 1rem auto;
		height: 2rem;
		background: white;
		color: black;
		font-size: 1.2rem;
		border-radius: 5px;
		/* display: flex;
		justify-content: space-between; */
	}
	.red__output-field p {
		padding: 5px 0 0 5px;
	}
	.red__faketext img {
		width: 75%;
		margin-left: 50px;
	}
	.red__lower-whitespace {
		margin-left: 1rem;
	}
	.red__lower-text {
		margin-right: 1rem;
	}
	.red__lower select {
		position: absolute;
		top: 7px;
		border: none;
		width: 100%;
	}

	@keyframes highlightField {
		0% {border-bottom: thin solid transparent;}
		50% {border-bottom: thin solid white;}
		100% {border-bottom: thin solid transparent;}
	}
</style>