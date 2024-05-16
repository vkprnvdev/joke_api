<script setup>
import { ref } from 'vue'
import axios from 'axios'

const nextBtn = ref(false)
const number = ref()
const firstString = ref()
const secondString = ref()
const loading = ref(false)

const refresh = () => {
	number.value = null
	firstString.value = null
	secondString.value = null
	nextBtn.value = false
}

const searchJoke = async () => {
	try {
		refresh()
		loading.value = true
		const { data } = await axios.get(
			'https://official-joke-api.appspot.com/random_joke/'
		)
		loading.value = false
		number.value = '№' + data.id
		firstString.value = data.setup
		nextBtn.value = true
		secondString.value = data.punchline
	} catch (err) {
		firstString.value = 'Error'
		console.log(err)
	}
}
</script>
<template>
	<div
		className="xl:w-[800px] max-h-[90%] rounded-2xl shadow-xl bg-slate-400 lg:w-[600px] sm:w-[500px]"
	>
		<h1 className="text-center p-3 text-3xl border-b border-slate-950">
			Jokes)
		</h1>
		<div className="flex flex-col items-center gap-3 pt-5 pb-5 w-full h-full">
			<button
				@click="searchJoke"
				className="relative w-2/5 border-2 border-transparent rounded-xl shadow-xl bg-slate-500 cursor-pointer transition  hover:shadow-none hover:text-white"
			>
				Generate
			</button>
			<div className="relative flex flex-col items-center">
				<img
					v-if="loading"
					className="mt-5"
					src="/loading.gif"
					alt="loading"
				/>
				<p className="mb-2">{{ number }}</p>
				<p className="max-w-[80%] text-center">{{ firstString }}</p>
				<button
					v-if="nextBtn"
					@click="nextBtn = !nextBtn"
					className="mt-2 text-slate-600 leading-none transition hover:text-white"
				>
					continue
				</button>
				<p v-if="!nextBtn" className="mt-2 max-w-[80%] text-center">
					{{ secondString }}
				</p>
			</div>
		</div>
	</div>
</template>
<style scoped></style>
