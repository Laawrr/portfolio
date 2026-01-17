<script lang="ts">
  const stats = [
    { number: "50+", label: "Projects Completed" },
    { number: "3+", label: "Years Experience" },
    { number: "100%", label: "Client Satisfaction" }
  ];
</script>

<section id="about" class="about-section">
  <div class="container">
    <div class="section-header">
      <h2 class="section-title">About Me</h2>
    </div>

    <div class="about-content">
      <div class="about-left">
        <div class="about-text">
          <p class="intro-text">
            I'm a passionate <span class="highlight">full-stack developer</span> with a keen eye for design 
            and user experience. I love building applications that are not only functional but also 
            beautiful and intuitive.
          </p>
          
          <p>
            With expertise in modern web technologies, I specialize in creating responsive,
            performant applications that deliver exceptional user experiences. I'm always
            learning new technologies and staying up-to-date with industry best practices.
          </p>
          
          <p>
            When I'm not coding, you can find me exploring new design trends, contributing to
            open-source projects, or sharing knowledge with the developer community.
          </p>
        </div>
      </div>

      <div class="about-right">
        <div class="stats-grid">
          {#each stats as stat}
            <div class="stat-card">
              <div class="stat-icon-wrapper">
                {#if stat.label === "Projects Completed"}
                  <svg class="stat-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M12 2L2 7L12 12L22 7L12 2Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M2 17L12 22L22 17" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M2 12L12 17L22 12" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                  </svg>
                {:else if stat.label === "Years Experience"}
                  <svg class="stat-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <rect x="3" y="7" width="18" height="14" rx="2" stroke="currentColor" stroke-width="2"/>
                    <path d="M8 7V5C8 3.89543 8.89543 3 10 3H14C15.1046 3 16 3.89543 16 5V7" stroke="currentColor" stroke-width="2"/>
                    <path d="M12 11V15" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
                  </svg>
                {:else}
                  <svg class="stat-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                  </svg>
                {/if}
              </div>
              <div class="stat-number">{stat.number}</div>
              <div class="stat-label">{stat.label}</div>
            </div>
          {/each}
        </div>
      </div>
    </div>
  </div>
</section>

<style>
  .about-section {
    padding: 5rem 0;
    position: relative;
    overflow: hidden;
    background: #000000;
  }

  .container {
    max-width: 1024px;
    margin: 0 auto;
    padding: 0 0.75rem;
  }

  .section-header {
    margin-bottom: 5rem;
  }

  .section-title {
    font-size: clamp(3rem, 6vw, 5rem);
    font-weight: 600;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin: 0;
    letter-spacing: -0.02em;
    line-height: 1.05;
  }

  .about-content {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
    align-items: start;
  }

  .about-text {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }

  .about-text p {
    font-size: 1.2rem;
    color: rgba(245, 245, 247, 0.8);
    line-height: 1.47059;
    margin: 0;
    font-weight: 400;
  }

  .intro-text {
    font-size: 1.4rem !important;
    color: #f5f5f7 !important;
    font-weight: 400;
  }

  .highlight {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    font-weight: 500;
  }

  .stats-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  .stat-card {
    text-align: left;
    padding: 2rem;
    background: rgba(245, 245, 247, 0.03);
    border-radius: 18px;
    border: 0.5px solid rgba(102, 126, 234, 0.2);
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    position: relative;
    overflow: hidden;
  }

  .stat-card::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 2px;
    background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
    transform: scaleX(0);
    transition: transform 0.4s;
  }

  .stat-card:hover {
    transform: translateY(-4px);
    background: rgba(245, 245, 247, 0.05);
    border-color: rgba(102, 126, 234, 0.4);
  }

  .stat-card:hover::before {
    transform: scaleX(1);
  }

  .stat-icon-wrapper {
    margin-bottom: 1rem;
  }

  .stat-icon {
    width: 32px;
    height: 32px;
    stroke: #667eea;
    fill: none;
    transition: all 0.3s;
  }

  .stat-card:hover .stat-icon {
    stroke: #764ba2;
    transform: scale(1.1);
  }

  .stat-number {
    font-size: 2.5rem;
    font-weight: 600;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 0.5rem;
    letter-spacing: -0.02em;
    line-height: 1;
  }

  .stat-label {
    color: rgba(245, 245, 247, 0.6);
    font-size: 0.95rem;
    font-weight: 400;
    letter-spacing: 0.01em;
  }

  @media (max-width: 768px) {
    .about-section {
      padding: 4rem 0;
    }

    .about-content {
      grid-template-columns: 1fr;
      gap: 4rem;
    }

    .section-header {
      margin-bottom: 3rem;
    }

    .section-title {
      font-size: 2.5rem;
    }

    .stats-grid {
      grid-template-columns: repeat(3, 1fr);
      gap: 1.5rem;
    }

    .stat-card {
      text-align: center;
      padding: 2rem 1.5rem;
    }

    .stat-number {
      font-size: 2.2rem;
    }

    .about-text p {
      font-size: 1.1rem;
    }

    .intro-text {
      font-size: 1.2rem !important;
    }
  }

  @media (max-width: 480px) {
    .about-section {
      padding: 3rem 0;
    }

    .container {
      padding: 0 1rem;
    }

    .section-title {
      font-size: 2rem;
    }

    .stats-grid {
      grid-template-columns: 1fr;
      gap: 1rem;
    }

    .stat-card {
      padding: 1.5rem;
    }

    .stat-number {
      font-size: 2rem;
    }
  }
</style>
