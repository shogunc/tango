<template>
	<div>
		<h1>Add Word</h1>
		<form>
			<div class="form-group">
				<label for="inputEnglish">Engelska</label>
				<input type="text" 
				       class="form-control" 
				       id="inputEnglish" 
				       v-model="inputData.english">
			</div>
			<div class="form-group">
				<label for="inputEnglishExample">Exempel (engelska)</label>
				<input type="text" 
				       class="form-control" 
				       id="inputEnglishExample" 
				       v-model="inputData.englishExample">
			</div>
			<div class="form-group">
				<label for="inputJapanese">Romaji</label>
				<input type="text" 
				       class="form-control" 
				       id="inputJapanese" 
				       v-model="inputData.japanese">
			</div>
			<div class="form-group">
				<label for="inputKana">Kana</label>
				<input type="text" 
				       class="form-control" 
				       id="inputKana" 
				       v-model="inputData.kana">
			</div>
			<div class="form-group">
				<label for="inputJapaneseExample">Exempel (japanska)</label>
				<input type="text" 
				       class="form-control" 
				       id="inputJapaneseExample" v-model="inputData.japaneseExample">
			</div>
		</form>
		<button type="submit" 
		        class="btn btn-secondary" 
		        @click=goHome>Tillbaka</button>
		<button type="submit" 
		        class="btn btn-primary" 
		        @click="submit">Lägg till</button>
	</div>
</template>

<script>
	export default {
		data () {
			return {
				inputData: {
					english: '',
					englishExample: '',
					japanese: '',
					japaneseExample: '',
					kana: ''	
				}
			}
		},
		methods: {
			submit: function () {
				console.log('Sending...')
				console.log(this.inputData.english)
			},
			goHome: function () {
				this.$router.push('/')
			}
		}
	}
</script>