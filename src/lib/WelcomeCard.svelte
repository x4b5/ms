<script>
  export let onStart;
</script>

<div class="glass-card welcome-card">
  <div class="icon-container">
    <div class="brain-icon">🧠</div>
  </div>
  <h1>De grote MS Quiz</h1>
  <p class="subtitle">Wat weet jij over MS?</p>

  <div class="info-box">
    <p>
      Ontdek handige feitjes over MS, hoe het werkt in je lichaam en hoe je het
      herkent.
    </p>
    <p class="stats">7 Vragen • Super Snel</p>
  </div>

  <button on:click={onStart} class="start-button">
    Start de Quiz
    <span class="arrow">→</span>
  </button>
</div>

<style>
  .welcome-card {
    text-align: center;
    background: linear-gradient(
      135deg,
      rgba(30, 41, 59, 0.8) 0%,
      rgba(15, 23, 42, 0.9) 100%
    );
  }

  .icon-container {
    font-size: 4rem;
    margin-bottom: 1rem;
    filter: drop-shadow(0 0 15px var(--primary-glow));
  }

  .subtitle {
    color: var(--text-muted);
    font-size: 1.2rem;
    margin-bottom: 2rem;
  }

  .info-box {
    background: rgba(255, 255, 255, 0.05);
    border-radius: 16px;
    padding: 1.5rem;
    margin-bottom: 2.5rem;
    border: 1px solid rgba(255, 255, 255, 0.05);
  }

  .stats {
    font-weight: 600;
    color: var(--accent);
    margin-top: 0.5rem;
    font-size: 0.9rem;
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .start-button {
    width: 100%;
    padding: 1.2rem;
    font-size: 1.2rem;
    gap: 10px;
  }

  .arrow {
    transition: transform 0.2s;
  }

  .start-button:hover .arrow {
    transform: translateX(5px);
  }
</style>
