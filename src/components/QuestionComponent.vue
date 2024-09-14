<template>
  <div>
    <h3>Question {{ index + 1 }}</h3>
    <p>{{ question.text }}</p>
    <textarea 
      :value="modelValue" 
      @input="$emit('update:modelValue', $event.target.value)"
      rows="5" 
      cols="50"
    ></textarea>
    <p v-if="question.type === 'theory'">You Got This</p>
    <p v-else-if="question.type === 'essay'">
      Minimum word count: 200 words
      <br>
      Current word count: {{ wordCount }}
    </p>
  </div>
</template>
  
<script>
  import { computed } from 'vue';
  
  export default {
    name: 'QuestionComponent',
    props: {
      question: {
        type: Object,
        required: true
      },
      index: {
        type: Number,
        required: true
      },
      modelValue: {
        type: String,
        default: ''
      }
    },
    emits: ['update:modelValue'],
    setup(props) {
      const wordCount = computed(() => {
        return props.modelValue.trim().split(/\s+/).length;
      });
  
      return {
        wordCount
      };
    }
  };
</script>
\end{code}

<style scoped>
.question {
  background-color: #e6e6fa;
  padding: 15px;
  border-radius: 10px;
  margin-bottom: 20px;
  transition: all 0.3s ease;
}

.question:hover {
  transform: translateY(-5px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

h3 {
  color: #8a2be2;
  margin-top: 0;
}

textarea {
  width: 100%;
  padding: 10px;
  border: 2px solid #dda0dd;
  border-radius: 5px;
  font-family: 'Poppins', sans-serif;
  resize: vertical;
}

.word-count {
  font-size: 0.9em;
  color: #696969;
  font-style: italic;
}
</style>