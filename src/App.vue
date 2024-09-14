<template>
  <div id="app">
    <h1>Computer Knowledge Theory Section</h1>
    <h3>Welcome Aasia</h3>
    <quiz-component v-if="!quizCompleted" @quiz-completed="onQuizCompleted"></quiz-component>
    <results-component v-else :score="score"></results-component>
  </div>
</template>

<script>
import QuizComponent from './components/QuizComponent.vue';
import ResultsComponent from './components/ResultsComponent.vue';

export default {
  name: 'App',
  components: {
    QuizComponent,
    ResultsComponent
  },
  data() {
    return {
      quizCompleted: false,
      score: 0
    };
  },
  methods: {
    onQuizCompleted(score) {
      this.score = score;
      this.quizCompleted = true;
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap');

body {
  background-color: #f0f8ff;
  margin: 0;
  padding: 0;
}

#app {
  font-family: 'Poppins', sans-serif;
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: white;
  border-radius: 20px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
}

.title {
  text-align: center;
  color: #ff69b4;
  font-size: 2.5em;
  margin-bottom: 30px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
  animation: bounce 1s ease infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}
</style>
