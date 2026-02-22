<script>
  export let score;
  export let total;
  export let onRestart;

  const percentage = Math.round((score / total) * 100);

  let message = "";
  let icon = "";

  if (percentage === 100) {
    message = "Super! Je bent een echte MS-expert.";
    icon = "🏆";
  } else if (percentage >= 70) {
    message = "Hartstikke goed gedaan! Je weet al heel veel.";
    icon = "🌟";
  } else if (percentage >= 50) {
    message = "Goed geprobeerd! Je hebt al een boel geleerd.";
    icon = "👍";
  } else {
    message =
      "Bedankt voor het meedoen! Je weet nu vast meer over MS dan eerst.";
    icon = "📚";
  }
</script>

<div class="glass-card result-card animate-fade-in">
  <div class="result-icon">{icon}</div>
  <h1>Quiz Voltooid!</h1>

  <div class="score-container">
    <div class="score-circle">
      <span class="score-val">{score}</span>
      <span class="score-total">/ {total}</span>
    </div>
    <p class="percentage">{percentage}% Correct</p>
  </div>

  <p class="result-message">{message}</p>

  <div class="actions">
    <button on:click={onRestart} class="restart-button"> Speel Opnieuw </button>
  </div>

  <p class="credits">
    Gemaakt door <strong>Raul</strong> en <strong>Xavier</strong>
  </p>
</div>

<style>
  .result-card {
    text-align: center;
  }

  .result-icon {
    font-size: 4rem;
    margin-bottom: 1rem;
  }

  .score-container {
    margin: 2rem 0;
  }

  .score-circle {
    display: inline-flex;
    align-items: baseline;
    justify-content: center;
    background: rgba(255, 255, 255, 0.05);
    width: 120px;
    height: 120px;
    border-radius: 50%;
    border: 4px solid var(--primary);
    padding: 20px;
    margin-bottom: 1rem;
    box-shadow: 0 0 20px var(--primary-glow);
  }

  .score-val {
    font-size: 3rem;
    font-weight: 700;
    color: var(--primary);
  }

  .score-total {
    font-size: 1.5rem;
    color: var(--text-muted);
    margin-left: 4px;
  }

  .percentage {
    font-weight: 600;
    color: var(--accent);
    font-size: 1.2rem;
  }

  .result-message {
    font-size: 1.1rem;
    margin-bottom: 2.5rem;
    color: var(--text-main);
  }

  .actions {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .restart-button {
    width: 100%;
    padding: 1rem;
  }

  .credits {
    margin-top: 2.5rem;
    font-size: 0.85rem;
    color: var(--text-muted);
    opacity: 0.8;
    border-top: 1px solid rgba(255, 255, 255, 0.05);
    padding-top: 1.5rem;
  }

  .credits strong {
    color: var(--accent);
  }
</style>
