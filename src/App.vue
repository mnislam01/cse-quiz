<template>
  <div id="app">
    
    <Header 
      v-bind:numCorrect="numCorrect"
      v-bind:numTotal="numTotal"
      v-bind:total="questions.length"
    />

    <br><br>

    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            v-bind:currentQuestion="questions[index]"
            v-bind:next="next"
            v-bind:increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>

  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    next: function() {
      this.index++
      if (this.index > this.questions.length){
        alert("Quiz Ended. Refresh the page to do it again.")
      }
    },
    increment: function(isCorrect) {
      if (isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=18&type=multiple', {
      method: 'get'
    }).then((response) => {
      return response.json()
    }).then((jsonData) => {
      this.questions = jsonData.results
    })
    }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
