<script>
	import { onMount } from 'svelte';

	let questions = [];
	let answers = []
	let userAnswers = [];
	
	onMount(async () => {
		const res = await fetch(`https://opentdb.com/api.php?amount=3&category=11&difficulty=medium&type=multiple`);
		const results = await res.json();
		questions = results.results
		console.log(questions)
	});



	function shuffleAnswers(incorrect_answers, correct_answer){

		answers.push(correct_answer)

		console.log(answers)
		let array = [...incorrect_answers,correct_answer]

		var currentIndex = array.length,  randomIndex;

		// While there remain elements to shuffle...
		while (0 !== currentIndex) {

		// Pick a remaining element...
		randomIndex = Math.floor(Math.random() * currentIndex);
		currentIndex--;

		// And swap it with the current element.
		[array[currentIndex], array[randomIndex]] = [
			array[randomIndex], array[currentIndex]];
		}

		return array
	}

	function showAnswers(){
		console.log(userAnswers)
}

	function saveAnswers(question_i, answer){

		let attribute = ("question "+question_i)
		userAnswers.push({
			attribute : answer
		})


		console.log(userAnswers)

	}


</script>

<main>
	<h1>Trivia Game</h1>
	<div>
		{#each questions as {question, incorrect_answers, correct_answer}, question_i}
			<p>{question}</p>
			<div>
				{#each shuffleAnswers(incorrect_answers, correct_answer) as answer, answers_i}
				<label >
					<input type=radio group={question_i} name={question_i} value={answers_i} on:click={()=> saveAnswers(question_i, answer)} >{answer}
				</label>
				{/each}
			</div>		
		{/each}
	</div>


	<button on:click={showAnswers}>Submit</button>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>