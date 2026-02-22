<script>
  export let question;
  export let questionNumber;
  export let totalQuestions;
  export let onAnswer;
  export let onNext;

  let selectedOption = null;
  let showedFeedback = false;
  let isCorrect = false;
  let firstAttempt = true;

  $: if (question) {
    selectedOption = null;
    showedFeedback = false;
    isCorrect = false;
    firstAttempt = true;
  }

  function selectOption(index) {
    if (showedFeedback) return;
    selectedOption = index;
    showedFeedback = true;
    isCorrect = index === question.correctAnswer;

    if (firstAttempt) {
      onAnswer(isCorrect, index);
      firstAttempt = false;
    }
  }

  function tryAgain() {
    selectedOption = null;
    showedFeedback = false;
    isCorrect = false;
  }

  const progress = (questionNumber / totalQuestions) * 100;
</script>

<div class="glass-card question-card">
  <div class="header">
    <span class="step-indicator"
      >Vraag {questionNumber} van {totalQuestions}</span
    >
    <div class="progress-bar">
      <div class="progress-fill" style="width: {progress}%"></div>
    </div>
  </div>

  <h2 class="question-text">{question.question}</h2>

  <div class="options-container">
    {#each question.options as option, i}
      <button
        class="option-button"
        class:selected={selectedOption === i}
        class:correct={showedFeedback &&
          isCorrect &&
          i === question.correctAnswer}
        class:incorrect={showedFeedback && !isCorrect && selectedOption === i}
        class:dimmed={showedFeedback && i !== selectedOption}
        on:click={() => selectOption(i)}
        disabled={showedFeedback}
      >
        <span class="option-label">{String.fromCharCode(65 + i)}</span>
        <span class="option-content">{option}</span>

        {#if showedFeedback && isCorrect && i === question.correctAnswer}
          <span class="feedback-icon">✓</span>
        {:else if showedFeedback && !isCorrect && selectedOption === i}
          <span class="feedback-icon">✕</span>
        {/if}
      </button>
    {/each}
  </div>

  {#if showedFeedback}
    <div class="feedback-section animate-fade-in">
      <div
        class="explanation-box"
        class:wrong={!isCorrect}
        class:correct={isCorrect}
      >
        <p>
          <strong>{isCorrect ? "Goed gedaan! 🌟" : "Bijna goed! 🤔"}</strong>
        </p>
        <p>
          {isCorrect
            ? question.explanation
            : "Dat was niet het juiste antwoord. Wil je het nog eens proberen?"}
        </p>
      </div>

      <div class="button-group">
        {#if !isCorrect}
          <button class="try-again-button" on:click={tryAgain}>
            Opnieuw Proberen
            <span class="retry-icon">↺</span>
          </button>
        {/if}

        <button
          class="next-button"
          class:secondary={!isCorrect}
          on:click={onNext}
        >
          {questionNumber === totalQuestions
            ? "Bekijk Resultaten"
            : "Volgende Vraag"}
          <span class="arrow">→</span>
        </button>
      </div>
    </div>
  {/if}
</div>

<style>
  .header {
    margin-bottom: 2rem;
  }

  .step-indicator {
    display: block;
    font-size: 0.9rem;
    color: var(--text-muted);
    margin-bottom: 0.5rem;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: 600;
  }

  .progress-bar {
    height: 6px;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 3px;
    overflow: hidden;
  }

  .progress-fill {
    height: 100%;
    background: linear-gradient(to right, var(--primary), var(--accent));
    transition: width 0.3s ease;
  }

  .question-text {
    font-size: 1.5rem;
    margin-bottom: 2rem;
    line-height: 1.4;
  }

  .options-container {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .option-button {
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 255, 255, 0.1);
    color: var(--text-main);
    justify-content: flex-start;
    text-align: left;
    padding: 1.2rem;
    box-shadow: none;
    transition: all 0.2s;
    position: relative;
    overflow: hidden;
  }

  .option-button:hover:not(:disabled) {
    background: rgba(255, 255, 255, 0.1);
    border-color: var(--primary);
    transform: translateX(4px);
  }

  .option-label {
    background: rgba(255, 255, 255, 0.1);
    width: 28px;
    height: 28px;
    border-radius: 6px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.85rem;
    margin-right: 15px;
    flex-shrink: 0;
  }

  .option-button.correct {
    background: rgba(16, 185, 129, 0.15);
    border-color: var(--success);
    color: white;
  }

  .option-button.correct .option-label {
    background: var(--success);
  }

  .option-button.incorrect {
    background: rgba(239, 68, 68, 0.15);
    border-color: var(--error);
  }

  .option-button.incorrect .option-label {
    background: var(--error);
  }

  .option-button.dimmed {
    opacity: 0.5;
  }

  .feedback-icon {
    margin-left: auto;
    font-weight: 700;
    font-size: 1.2rem;
  }

  .feedback-section {
    margin-top: 2rem;
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }

  .explanation-box {
    padding: 1.2rem;
    background: rgba(255, 255, 255, 0.05);
    border-left: 4px solid var(--text-muted);
    border-radius: 8px;
    font-size: 0.95rem;
    color: #e2e8f0;
  }

  .explanation-box.correct {
    background: rgba(16, 185, 129, 0.1);
    border-left-color: var(--success);
  }

  .explanation-box.wrong {
    background: rgba(239, 68, 68, 0.1);
    border-left-color: var(--error);
  }

  .button-group {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .try-again-button {
    width: 100%;
    padding: 1rem;
    font-size: 1.1rem;
    gap: 8px;
    background-color: var(--secondary);
    box-shadow: 0 4px 12px rgba(99, 102, 241, 0.3);
  }

  .next-button {
    width: 100%;
    padding: 1rem;
    font-size: 1.1rem;
    gap: 8px;
    background: linear-gradient(135deg, var(--primary) 0%, #f16d26 100%);
  }

  .next-button.secondary {
    background: rgba(255, 255, 255, 0.05);
    box-shadow: none;
    color: var(--text-muted);
    font-size: 0.95rem;
  }

  .arrow,
  .retry-icon {
    transition: transform 0.2s;
  }

  .next-button:hover .arrow {
    transform: translateX(4px);
  }

  .try-again-button:hover .retry-icon {
    transform: rotate(-180deg);
  }
</style>
