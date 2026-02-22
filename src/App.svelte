<script>
  import { questions } from './data/questions.js';
  import QuestionCard from './lib/QuestionCard.svelte';
  import ResultCard from './lib/ResultCard.svelte';
  import WelcomeCard from './lib/WelcomeCard.svelte';

  let currentState = 'WELCOME'; // WELCOME, QUIZ, RESULT
  let currentQuestionIndex = 0;
  let score = 0;
  let answers = [];

  function startQuiz() {
    currentState = 'QUIZ';
    currentQuestionIndex = 0;
    score = 0;
    answers = [];
  }

  function handleAnswer(isCorrect, answerIndex) {
    if (isCorrect) score++;
    answers = [...answers, answerIndex];
    
    setTimeout(() => {
      if (currentQuestionIndex < questions.length - 1) {
        currentQuestionIndex++;
      } else {
        currentState = 'RESULT';
      }
    }, 1500); // Give time to see feedback
  }

  function restartQuiz() {
    startQuiz();
  }
</script>

<main class="animate-fade-in">
  <div class="container">
    {#if currentState === 'WELCOME'}
      <WelcomeCard onStart={startQuiz} />
    {:else if currentState === 'QUIZ'}
      <QuestionCard 
        question={questions[currentQuestionIndex]} 
        questionNumber={currentQuestionIndex + 1}
        totalQuestions={questions.length}
        onAnswer={handleAnswer}
      />
    {:else if currentState === 'RESULT'}
      <ResultCard 
        {score} 
        total={questions.length} 
        onRestart={restartQuiz} 
      />
    {/if}
  </div>
</main>

<style>
  .container {
    width: 100%;
    margin-top: 2rem;
  }
</style>
