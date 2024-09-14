<!-- src/components/ResultsComponent.vue -->
<template>
    <div class="results">
      <h2>Quiz Results</h2>
      <p class="score">Your score: {{ score }} out of 50</p>
      <div class="celebration" v-if="score >= 25">
        <span class="emoji">🎉</span>
        <span class="emoji">🌟</span>
        <span class="emoji">🎊</span>
      </div>
      <p class="message" v-if="score >= 25">Great job! You did amazing!</p>
      <p class="message" v-else>Keep practicing! You'll do better next time!</p>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ResultsComponent',
    props: {
      score: {
        type: Number,
        required: true
      }
    }
  };
  </script>
  
  <style scoped>
  .results {
    background-color: #fffaf0;
    padding: 30px;
    border-radius: 15px;
    text-align: center;
    animation: scaleIn 0.5s ease-out;
  }
  
  @keyframes scaleIn {
    from { transform: scale(0.8); opacity: 0; }
    to { transform: scale(1); opacity: 1; }
  }
  
  h2 {
    color: #ff69b4;
    font-size: 2em;
    margin-bottom: 20px;
  }
  
  .score {
    font-size: 1.5em;
    color: #4b0082;
    font-weight: bold;
  }
  
  .celebration {
    margin: 20px 0;
  }
  
  .emoji {
    font-size: 3em;
    margin: 0 10px;
    animation: spin 2s linear infinite;
    display: inline-block;
  }
  
  @keyframes spin {
    100% { transform: rotate(360deg); }
  }
  
  .message {
    font-size: 1.2em;
    color: #333;
    margin-top: 20px;
  }
  </style>