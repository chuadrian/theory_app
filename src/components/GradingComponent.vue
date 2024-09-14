<template>
  <div>
    <h2>Grading Interface</h2>
    <div v-for="(question, index) in questions" :key="index">
      <h3>Question {{ index + 1 }}</h3>
      <p><strong>Question:</strong> {{ question.text }}</p>
      <p><strong>Student's Answer:</strong> {{ answers[index] }}</p>
      <label>
        Score (0-{{ question.type === 'theory' ? '4' : '10' }}):
        <input 
          type="number" 
          v-model.number="grades[index]" 
          :min="0" 
          :max="question.type === 'theory' ? 4 : 10"
        >
      </label>
    </div>
    <button @click="submitGrades">Submit Grades</button>
  </div>
</template>

<script>
import { ref, watch } from 'vue';

export default {
  name: 'GradingComponent',
  props: {
    questions: {
      type: Array,
      required: true
    },
    answers: {
      type: Array,
      required: true
    }
  },
  emits: ['grading-completed'],
  setup(props, { emit }) {
    const grades = ref(props.questions.map(() => 0));

    // Reset grades when questions change
    watch(() => props.questions, () => {
      grades.value = props.questions.map(() => 0);
    });

    const submitGrades = () => {
      const totalScore = grades.value.reduce((sum, grade) => sum + grade, 0);
      emit('grading-completed', totalScore);
      console.log('Grades submitted. Total score:', totalScore);
    };

    return {
      grades,
      submitGrades
    };
  }
};
</script>

<style scoped>
.grading {
  background-color: #f0fff0;
  padding: 20px;
  border-radius: 15px;
  animation: slideIn 0.5s ease-out;
}

@keyframes slideIn {
  from { transform: translateY(-20px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

.grade-item {
  background-color: #fff;
  padding: 15px;
  margin-bottom: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

h2, h3 {
  color: #20b2aa;
}

input[type="number"] {
  width: 60px;
  padding: 5px;
  border: 2px solid #20b2aa;
  border-radius: 5px;
}

.submit-grades {
  background-color: #20b2aa;
  color: white;
  border: none;
  padding: 10px 20px;
  font-size: 1em;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.submit-grades:hover {
  background-color: #48d1cc;
  transform: scale(1.05);
}
</style>