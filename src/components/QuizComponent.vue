<template>
    <div>
      <div v-if="!quizStarted && !quizCompleted">
        <button @click="startQuiz">Start Test</button>
      </div>
      <div v-else-if="quizStarted && !quizCompleted">
        <timer-component :time-remaining="timeRemaining"></timer-component>
        <div v-for="(question, index) in questions" :key="index">
          <question-component
            :question="question"
            :index="index"
            :modelValue="answers[index]"
            @update:modelValue="updateAnswer(index, $event)"
          ></question-component>
        </div>
        <button @click="submitQuiz" :disabled="!allQuestionsAnswered">Submit Quiz</button>
      </div>
      <grading-component 
        v-if="quizCompleted"
        :questions="questions" 
        :answers="answers" 
        @grading-completed="onGradingCompleted"
      ></grading-component>
      <div v-if="quizCompleted && finalScore !== null">
        <h2>Quiz Completed</h2>
        <p>Your final score is: {{ finalScore }}</p>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, computed } from 'vue';
  import TimerComponent from './TimerComponent.vue';
  import QuestionComponent from './QuestionComponent.vue';
  import GradingComponent from './GradingComponent.vue';
  import { questions } from '../data/questions';
  
  export default {
    name: 'QuizComponent',
    components: {
      TimerComponent,
      QuestionComponent,
      GradingComponent
    },
    setup() {
      const quizStarted = ref(false);
      const quizCompleted = ref(false);
      const timeRemaining = ref(45 * 60);
      const timer = ref(null);
      const answers = ref(Array(questions.length).fill(''));
      const finalScore = ref(null);
  
      const startQuiz = () => {
        quizStarted.value = true;
        timer.value = setInterval(updateTimer, 1000);
      };
  
      const updateTimer = () => {
        if (timeRemaining.value > 0) {
          timeRemaining.value--;
        } else {
          submitQuiz();
        }
      };
  
      const submitQuiz = () => {
        clearInterval(timer.value);
        quizCompleted.value = true;
      };
  
      const onGradingCompleted = (score) => {
        finalScore.value = score;
        console.log(`Quiz completed. Final score: ${score}`);
      };
  
      const updateAnswer = (index, value) => {
        answers.value[index] = value;
      };
  
      const wordCount = (str) => {
        return str.trim().split(/\s+/).length;
      };
  
      const allQuestionsAnswered = computed(() => {
        return answers.value.every((answer, index) => {
          const question = questions[index];
          if (question.type === 'essay') {
            return wordCount(answer) >= 200;
          }
          return answer.trim() !== '';
        });
      });
  
      return {
        quizStarted,
        quizCompleted,
        timeRemaining,
        questions,
        answers,
        finalScore,
        startQuiz,
        submitQuiz,
        onGradingCompleted,
        updateAnswer,
        allQuestionsAnswered
      };
    }
  };
  </script>

<style scoped>
.quiz-container {
  background-color: #fff0f5;
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 0 15px rgba(255, 105, 180, 0.2);
}

.start-screen {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;
}

.start-button, .submit-button {
  background-color: #ff69b4;
  color: white;
  border: none;
  padding: 15px 30px;
  font-size: 1.2em;
  border-radius: 25px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.start-button:hover, .submit-button:hover {
  background-color: #ff1493;
  transform: scale(1.05);
}

.quiz-content {
  animation: fadeIn 0.5s ease-out;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

.question-container {
  margin-bottom: 20px;
}

.submit-button:disabled {
  background-color: #d3d3d3;
  cursor: not-allowed;
}
</style>
