<template>
	<div class="question-box-container">
		<b-jumbotron>
			<template #lead>
				<p v-html="currentQuestion.question"></p>
			</template>

			<hr class="my-4">

			<b-list-group 
				v-for="(answer, index) in shuffledAnswers" 
				v-bind:key="index"
				v-on:click="selectAnswer(index)"
			>
				<b-list-group-item 
					v-bind:class="answerClass(index)"
				>
					{{ answer }}
				</b-list-group-item>
			</b-list-group>

			<b-button 
				v-on:click="submitAnswer" 
				variant="primary"
				v-bind:disabled="selectedIndex === null || answered"
			>
				Submit
			</b-button>
			<b-button 
				v-on:click="next" 
				variant="success"
			>
				Next
			</b-button>
		</b-jumbotron>
	</div>
</template>

<script>
import _ from 'lodash';

export default {
	props: {
		currentQuestion: Object,
		next: Function,
		increament: Function
	},
	data: function() {
		return {
			selectedIndex: null,
			correctIndex: null,
			shuffledAnswers: [],
			answered: false
		}
	},
	// computed: {
	// 	answers() {
	// 		let answers = [...this.currentQuestion.incorrect_answers];
	// 		answers.push(this.currentQuestion.correct_answer);
	// 		return answers;
	// 	}
	// },
	watch: {
		currentQuestion: {
			immediate: true,
			handler() {
				this.selectedIndex = null;
				this.answered =false;
				this.shuffleAnswers();
			}
		}
	},
	methods: {
		selectAnswer(index) {
			this.selectedIndex = index;
		},
		submitAnswer() {
			let isCorrect = false;

			if(this.selectedIndex === this.correctIndex) {
				isCorrect = true;
			}

			this.answered = true;
			this.increament(isCorrect);
		},
		shuffleAnswers() {
			let shuffle_answers = [...this.currentQuestion.incorrect_answers];
			shuffle_answers.push(this.currentQuestion.correct_answer);
			this.shuffledAnswers = _.shuffle(shuffle_answers);
			this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
		},
		answerClass(index) {
			let answerClass = '';

			if(!this.answered && this.selectedIndex === index) {
				answerClass = 'selected';
			}
			else if(this.answered && this.correctIndex === index) {
				answerClass = 'correct';
			}
			else if(this.answered && this.selectedIndex === index && this.correctIndex !== index) {
				answerClass = 'incorrect';
			}

			return answerClass;
		}
	}
}
</script>

<style scoped>
.question-box-container {
	margin-top: 15px;
	padding: 15px;
	background-color: #e9ecef;
	border-radius: 5px;
}
.list-group {
	margin-bottom: 5px;
	background-color: white;
}
.list-group-item:hover {
	background-color: lightblue;
	cursor: pointer;
}
.btn {
	margin: 5px 5px;
}
.selected {
	background-color: lightblue;
}
.correct {
	background-color: lightgreen;
}
.incorrect {
	background-color: red;
}
</style>