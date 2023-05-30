<script>
  let questions = [
    {
      question: 'What is the capital of France?',
      options: ['Paris', 'London', 'Madrid', 'Berlin'],
      correctAnswer: 'Paris',
      selectedAnswer: ''
    },
    {
      question: 'Which planet is known as the Red Planet?',
      options: ['Mars', 'Venus', 'Jupiter', 'Mercury'],
      correctAnswer: 'Mars',
      selectedAnswer: ''
    },
    {
      question: 'What is the largest ocean in the world?',
      options: ['Pacific Ocean', 'Atlantic Ocean', 'Indian Ocean', 'Arctic Ocean'],
      correctAnswer: 'Pacific Ocean',
      selectedAnswer: ''
    }
  ];

  let currentQuestionIndex = 0;
  let score = 0;

  function selectAnswer(option) {
    questions[currentQuestionIndex].selectedAnswer = option;
  }

  function nextQuestion() {
    if (currentQuestionIndex < questions.length - 1) {
      currentQuestionIndex++;
    } else {
      calculateScore();
    }
  }

  function calculateScore() {
    score = questions.reduce((totalScore, question) => {
      if (question.selectedAnswer === question.correctAnswer) {
        return totalScore + 1;
      } else {
        return totalScore;
      }
    }, 0);
  }
</script>

<style>
  /* Add styling for the quiz app */
</style>

{#if currentQuestionIndex < questions.length}
  <h2>Question {currentQuestionIndex + 1}</h2>
  <p>{questions[currentQuestionIndex].question}</p>

  <ul>
    {#each questions[currentQuestionIndex].options as option}
      <li on:click={() => selectAnswer(option)}>{option}</li>
    {/each}
  </ul>

  <button on:click={nextQuestion}>Next</button>
{:else}
  <h2>Quiz Completed</h2>
  <p>Your Score: {score}/{questions.length}</p>
{/if}

