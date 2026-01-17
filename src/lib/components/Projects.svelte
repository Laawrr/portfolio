<script lang="ts">
  const projects = [
    {
      title: "E-Commerce Platform",
      description: "A full-stack e-commerce solution with payment integration, user authentication, and admin dashboard.",
      tech: ["React", "Node.js", "MongoDB", "Stripe"],
      link: "#",
      category: "Full Stack"
    },
    {
      title: "Task Management App",
      description: "A collaborative task management application with real-time updates and team collaboration features.",
      tech: ["Svelte", "Firebase", "TypeScript"],
      link: "#",
      category: "Frontend"
    },
    {
      title: "Weather Dashboard",
      description: "Beautiful weather dashboard with location-based forecasts and interactive maps.",
      tech: ["Vue.js", "API Integration", "Chart.js"],
      link: "#",
      category: "Frontend"
    },
    {
      title: "Portfolio Website",
      description: "Modern, responsive portfolio website with smooth animations and dark mode support.",
      tech: ["SvelteKit", "GSAP", "TypeScript"],
      link: "#",
      category: "Frontend"
    },
    {
      title: "Social Media Analytics",
      description: "Comprehensive analytics platform for social media metrics with real-time data visualization.",
      tech: ["React", "Python", "PostgreSQL", "D3.js"],
      link: "#",
      category: "Full Stack"
    },
    {
      title: "Fitness Tracker App",
      description: "Mobile-first fitness tracking application with workout plans and progress tracking.",
      tech: ["React Native", "Firebase", "Redux"],
      link: "#",
      category: "Mobile"
    }
  ];
</script>

<section id="projects" class="projects-section">
  <div class="container">
    <div class="section-header">
      <h2 class="section-title">Featured Projects</h2>
    </div>

    <div class="projects-grid">
      {#each projects as project}
        <div class="project-card">
          <div class="project-content">
            <div class="project-header">
              <span class="project-category">{project.category}</span>
            </div>
            
            <h3 class="project-title">{project.title}</h3>
            <p class="project-description">{project.description}</p>
            
            <div class="project-tech">
              {#each project.tech as tech}
                <span class="tech-tag">{tech}</span>
              {/each}
            </div>
            
            <div class="project-actions">
              <a href={project.link} class="project-link">
                <span>Learn more</span>
                <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path d="M6 12L10 8L6 4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
              </a>
            </div>
          </div>
        </div>
      {/each}
    </div>
  </div>
</section>

<style>
  .projects-section {
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

  .projects-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 1.5rem;
  }

  .project-card {
    background: rgba(245, 245, 247, 0.03);
    border: 0.5px solid rgba(102, 126, 234, 0.2);
    border-radius: 18px;
    overflow: hidden;
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    position: relative;
  }

  .project-card::before {
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

  .project-card:hover {
    transform: translateY(-4px);
    background: rgba(245, 245, 247, 0.05);
    border-color: rgba(102, 126, 234, 0.4);
  }

  .project-card:hover::before {
    transform: scaleX(1);
  }

  .project-content {
    padding: 2.5rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .project-header {
    margin-bottom: 0.5rem;
  }

  .project-category {
    padding: 0.4rem 0.9rem;
    background: rgba(102, 126, 234, 0.15);
    color: #667eea;
    border-radius: 12px;
    font-size: 0.75rem;
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    border: 0.5px solid rgba(102, 126, 234, 0.3);
    display: inline-block;
  }

  .project-title {
    font-size: 1.5rem;
    font-weight: 600;
    color: #f5f5f7;
    margin: 0;
    letter-spacing: -0.01em;
    line-height: 1.2;
  }

  .project-description {
    color: rgba(245, 245, 247, 0.7);
    line-height: 1.47059;
    margin: 0;
    font-size: 1.05rem;
    font-weight: 400;
    flex: 1;
  }

  .project-tech {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-top: 0.5rem;
  }

  .tech-tag {
    padding: 0.4rem 0.9rem;
    background: rgba(245, 245, 247, 0.08);
    color: rgba(245, 245, 247, 0.8);
    border-radius: 10px;
    font-size: 0.85rem;
    border: 0.5px solid rgba(255, 255, 255, 0.1);
    font-weight: 400;
    letter-spacing: -0.01em;
  }

  .project-actions {
    margin-top: 1rem;
    padding-top: 1rem;
    border-top: 0.5px solid rgba(102, 126, 234, 0.2);
  }

  .project-link {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    color: #667eea;
    text-decoration: none;
    font-weight: 400;
    font-size: 1.05rem;
    transition: all 0.3s;
    letter-spacing: -0.01em;
  }

  .project-link:hover {
    color: #764ba2;
    gap: 0.75rem;
  }

  .project-link svg {
    transition: transform 0.3s;
  }

  .project-link:hover svg {
    transform: translateX(3px);
  }

  @media (max-width: 768px) {
    .projects-section {
      padding: 4rem 0;
    }

    .projects-grid {
      grid-template-columns: 1fr;
      gap: 1.5rem;
    }

    .section-header {
      margin-bottom: 3rem;
    }

    .section-title {
      font-size: 2.5rem;
    }

    .project-content {
      padding: 2rem;
    }
  }

  @media (max-width: 480px) {
    .projects-section {
      padding: 3rem 0;
    }

    .container {
      padding: 0 1rem;
    }

    .section-title {
      font-size: 2rem;
    }

    .project-content {
      padding: 1.5rem;
    }

    .project-title {
      font-size: 1.3rem;
    }
  }
</style>
