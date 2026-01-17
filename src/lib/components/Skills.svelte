<script lang="ts">
  const skills = [
    { name: "JavaScript", level: 90 },
    { name: "TypeScript", level: 85 },
    { name: "React", level: 88 },
    { name: "Svelte", level: 82 },
    { name: "Node.js", level: 80 },
    { name: "CSS/SCSS", level: 92 },
    { name: "Git", level: 85 },
    { name: "UI/UX Design", level: 75 }
  ];

  const techCategories = [
    {
      title: "Frontend",
      technologies: ["React", "Svelte", "Vue.js", "TypeScript", "HTML5", "CSS3", "Tailwind"]
    },
    {
      title: "Backend",
      technologies: ["Node.js", "Express", "MongoDB", "PostgreSQL", "REST APIs", "GraphQL"]
    },
    {
      title: "Tools & Others",
      technologies: ["Git", "Docker", "Webpack", "Vite", "Figma", "Adobe XD"]
    }
  ];
</script>

<section id="skills" class="skills-section">
  <div class="container">
    <div class="section-header">
      <h2 class="section-title">Skills & Technologies</h2>
    </div>

    <div class="skills-content">
      <div class="skills-left">
        <div class="skills-list">
          {#each skills as skill}
            <div class="skill-item">
              <div class="skill-header">
                <span class="skill-name">{skill.name}</span>
                <span class="skill-percent">{skill.level}%</span>
              </div>
              <div class="skill-bar-container">
                <div class="skill-bar">
                  <div class="skill-progress" style="width: {skill.level}%"></div>
                </div>
              </div>
            </div>
          {/each}
        </div>
      </div>

      <div class="skills-right">
        <div class="categories-grid">
          {#each techCategories as category}
            <div class="category-card">
              <h4 class="category-title">{category.title}</h4>
              <div class="tech-tags">
                {#each category.technologies as tech}
                  <span class="tech-tag">{tech}</span>
                {/each}
              </div>
            </div>
          {/each}
        </div>
      </div>
    </div>
  </div>
</section>

<style>
  .skills-section {
    padding: 5rem 0;
    background: #000000;
    position: relative;
    overflow: hidden;
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

  .skills-content {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
    align-items: start;
  }

  .skills-list {
    display: grid;
    gap: 1.5rem;
  }

  .skill-item {
    background: rgba(245, 245, 247, 0.03);
    border: 0.5px solid rgba(102, 126, 234, 0.2);
    border-radius: 18px;
    padding: 1.75rem;
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
  }

  .skill-item:hover {
    background: rgba(245, 245, 247, 0.05);
    border-color: rgba(102, 126, 234, 0.4);
    transform: translateY(-2px);
  }

  .skill-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
  }

  .skill-name {
    font-weight: 500;
    color: #f5f5f7;
    font-size: 1.1rem;
    letter-spacing: -0.01em;
  }

  .skill-percent {
    color: rgba(245, 245, 247, 0.6);
    font-weight: 400;
    font-size: 0.95rem;
  }

  .skill-bar-container {
    position: relative;
  }

  .skill-bar {
    height: 8px;
    background: rgba(102, 126, 234, 0.1);
    border-radius: 4px;
    overflow: hidden;
    position: relative;
  }

  .skill-progress {
    height: 100%;
    background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
    border-radius: 4px;
    transition: width 1s cubic-bezier(0.4, 0, 0.2, 1);
    position: relative;
  }

  .categories-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  .category-card {
    background: rgba(245, 245, 247, 0.03);
    border: 0.5px solid rgba(102, 126, 234, 0.2);
    border-radius: 18px;
    padding: 2rem;
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
  }

  .category-card:hover {
    transform: translateY(-4px);
    background: rgba(245, 245, 247, 0.05);
    border-color: rgba(102, 126, 234, 0.4);
  }

  .category-title {
    font-size: 1.3rem;
    font-weight: 600;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin: 0 0 1.5rem 0;
    letter-spacing: -0.01em;
  }

  .tech-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }

  .tech-tag {
    padding: 0.5rem 1rem;
    background: rgba(102, 126, 234, 0.15);
    color: #667eea;
    border-radius: 12px;
    font-size: 0.9rem;
    border: 0.5px solid rgba(102, 126, 234, 0.3);
    transition: all 0.3s;
    font-weight: 400;
    letter-spacing: -0.01em;
  }

  .tech-tag:hover {
    background: rgba(102, 126, 234, 0.25);
    transform: scale(1.05);
  }

  @media (max-width: 768px) {
    .skills-section {
      padding: 4rem 0;
    }

    .skills-content {
      grid-template-columns: 1fr;
      gap: 4rem;
    }

    .section-header {
      margin-bottom: 3rem;
    }

    .section-title {
      font-size: 2.5rem;
    }

    .skill-item {
      padding: 1.5rem;
    }

    .categories-grid {
      grid-template-columns: 1fr;
      gap: 1.5rem;
    }

    .category-card {
      padding: 2rem;
    }
  }

  @media (max-width: 480px) {
    .skills-section {
      padding: 3rem 0;
    }

    .container {
      padding: 0 1rem;
    }

    .section-title {
      font-size: 2rem;
    }

    .category-card {
      padding: 1.5rem;
    }
  }
</style>
