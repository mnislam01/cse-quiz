<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        <p v-html="currentQuestion.question"></p>
      </template>

      <hr class="my-4" />
      <b-list-group v-for="(ans, i) in shuffledAnswers" v-bind:key="i">
        <b-list-group-item
          v-on:click="selectAnswer(i)"
          v-bind:class="[
            !answered && selectedIndx === i ? 'selected': 
            answered && correctIndx === i ? 'correct': 
            answered && selectedIndx == i && correctIndx !== i ? 'incorrect': ''
          ]"
        >
            <p v-html="ans"></p>
        </b-list-group-item>
      </b-list-group>

      <b-button 
        variant="primary" 
        v-on:click="submitAnswer"
        v-bind:disabled="selectedIndx === null || answered"
      >
        Submit
      </b-button>
      <b-button variant="success" v-on:click="next" v-bind:disabled="answered === false">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndx: null,
      correctIndx: null,
      shuffledAnswers: [],
      answered: false
    };
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndx = null
        this.answered = false
        this.shuffleAnswers()
      }
    }
  },
  methods: {
    selectAnswer: function(i) {
      this.selectedIndx = i;
    },
    shuffleAnswers: function() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndx = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer: function() {
        let isCorrent = false
        if (this.selectedIndx === this.correctIndx) {
            isCorrent = true
        }

        this.answered = true

        this.increment(isCorrent)
    }
  },
  computed: {
    answers: function() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: lightcoral;
}

.btn {
  margin: 0 5px;
}
</style>