<script lang="ts">
  import { onMount, tick } from "svelte";
  import gsap from "gsap";
  import ScrollTrigger from "gsap/ScrollTrigger";
  import About from "$lib/components/About.svelte";
  import Skills from "$lib/components/Skills.svelte";
  import Projects from "$lib/components/Projects.svelte";
  import Contact from "$lib/components/Contact.svelte";
  import heroImage from "$lib/assets/example.png";

  let intro: HTMLElement;
  let content: HTMLElement;
  let heroText: HTMLElement;
  let logoLetters: HTMLElement;

  let started = false;
  let lettersLoaded = 0;
  let mobileMenuOpen = false;
  let showTapPrompt = false;
  const totalLetters = 8; // L-A-W-R-E-N-C-E

  let heroVisualInner: HTMLElement;
  let heroVisualContainer: HTMLElement;
  
  let mouseX = 0;
  let mouseY = 0;
  let scrollY = 0;
  let targetRotateX = 0;
  let targetRotateY = 0;
  let currentRotateX = 0;
  let currentRotateY = 0;
  let isAnimating = false;
  let cachedRect: DOMRect | null = null;
  let rectUpdateTime = 0;
  const RECT_CACHE_DURATION = 100; // Update rect cache every 100ms

  // Throttle mouse move
  let mouseMoveTimeout: number | null = null;
  function handleGlobalMouseMove(e: MouseEvent) {
    mouseX = e.clientX;
    mouseY = e.clientY;
    
    // Throttle updates
    if (mouseMoveTimeout) return;
    mouseMoveTimeout = window.setTimeout(() => {
      mouseMoveTimeout = null;
    }, 16); // ~60fps
    
    if (!isAnimating) {
      isAnimating = true;
      requestAnimationFrame(update3DTransform);
    }
  }

  // Throttle scroll
  let scrollTimeout: number | null = null;
  function handleScroll() {
    scrollY = window.scrollY;
    
    // Throttle updates
    if (scrollTimeout) return;
    scrollTimeout = window.setTimeout(() => {
      scrollTimeout = null;
    }, 16); // ~60fps
    
    if (!isAnimating) {
      isAnimating = true;
      requestAnimationFrame(update3DTransform);
    }
  }

  function update3DTransform() {
    if (!heroVisualContainer || !heroVisualInner) {
      isAnimating = false;
      return;
    }
    
    // Cache getBoundingClientRect - only update every 100ms
    const now = Date.now();
    if (!cachedRect || now - rectUpdateTime > RECT_CACHE_DURATION) {
      cachedRect = heroVisualContainer.getBoundingClientRect();
      rectUpdateTime = now;
    }
    
    const rect = cachedRect;
    const centerX = rect.left + rect.width / 2;
    const centerY = rect.top + rect.height / 2;
    
    // Mouse-based rotation (reduced intensity for smoother performance)
    const mouseOffsetX = mouseX - centerX;
    const mouseOffsetY = mouseY - centerY;
    const mouseRotateY = (mouseOffsetX / window.innerWidth) * 20;
    const mouseRotateX = -(mouseOffsetY / window.innerHeight) * 20;
    
    // Scroll-based rotation (reduced intensity)
    const scrollRotateY = (scrollY / window.innerHeight) * 10;
    const scrollRotateX = (scrollY / window.innerHeight) * 8;
    
    // Combine mouse and scroll rotations
    targetRotateX = mouseRotateX + scrollRotateX;
    targetRotateY = mouseRotateY + scrollRotateY;
    
    // Smooth interpolation with faster easing
    const ease = 0.15;
    currentRotateX += (targetRotateX - currentRotateX) * ease;
    currentRotateY += (targetRotateY - currentRotateY) * ease;
    
    // Only update if change is significant (reduce repaints)
    const threshold = 0.1;
    if (Math.abs(currentRotateX) > threshold || Math.abs(currentRotateY) > threshold || 
        Math.abs(targetRotateX) > threshold || Math.abs(targetRotateY) > threshold) {
      // Use transform3d for hardware acceleration
      heroVisualInner.style.transform = `translate3d(0, 0, 0) perspective(1000px) rotateX(${currentRotateX}deg) rotateY(${currentRotateY}deg)`;
      
      // Continue animation if still moving
      if (Math.abs(targetRotateX - currentRotateX) > 0.01 || Math.abs(targetRotateY - currentRotateY) > 0.01) {
        requestAnimationFrame(update3DTransform);
      } else {
        isAnimating = false;
      }
    } else {
      isAnimating = false;
    }
  }

  function toggleMobileMenu() {
    mobileMenuOpen = !mobileMenuOpen;
  }

  function closeMobileMenu() {
    mobileMenuOpen = false;
  }

  onMount(() => {
    // Register GSAP plugins (browser-only)
    gsap.registerPlugin(ScrollTrigger);
    
    // Set up 3D mouse and scroll tracking
    window.addEventListener('mousemove', handleGlobalMouseMove, { passive: true });
    window.addEventListener('scroll', handleScroll, { passive: true });
    
    // Hide portfolio initially
    gsap.set(content, {
      opacity: 0,
      scale: 0.95,
      filter: "blur(10px)"
    });

    // Wait for DOM to be ready
    tick().then(() => {
      // Animate logo letters one by one (loading effect)
      if (logoLetters) {
        const letters = Array.from(logoLetters.children) as HTMLElement[];
        letters.forEach((letter: HTMLElement, index: number) => {
          // Skip animation for space character, but still count it
          const text = letter.textContent?.trim() || '';
          if (text === '' || text === '\u00A0') {
            gsap.set(letter, { opacity: 1, y: 0 });
            lettersLoaded++;
            if (lettersLoaded === totalLetters) {
              showTapPrompt = true;
            }
      return;
    }

          gsap.set(letter, { opacity: 0, y: 30 });
          gsap.to(letter, {
            opacity: 1,
            y: 0,
            duration: 0.3,
            delay: index * 0.15,
            ease: "power2.out",
            onComplete: () => {
              // Clear GSAP transform so CSS animation can take over
              gsap.set(letter, { clearProps: "transform" });
              // Add class to mark letter as loaded (but don't animate yet)
              letter.classList.add('letter-loaded');
              lettersLoaded++;
              if (lettersLoaded === totalLetters) {
                showTapPrompt = true;
              }
            }
          });
        });
      }
    });
    
    // Cleanup
    return () => {
      window.removeEventListener('mousemove', handleGlobalMouseMove);
      window.removeEventListener('scroll', handleScroll);
      if (mouseMoveTimeout) clearTimeout(mouseMoveTimeout);
      if (scrollTimeout) clearTimeout(scrollTimeout);
      isAnimating = false;
    };
  });

  function startPortfolio() {
    if (started || lettersLoaded < totalLetters) return;
    started = true;

    const tl = gsap.timeline();

    tl.to(intro, {
      scale: 1.1,
      opacity: 0,
      filter: "blur(20px)",
      duration: 1,
      ease: "power3.inOut"
    })
    .set(intro, { display: "none" })
    .call(() => {
      revealPortfolio(true); // true = animate letters
    });
  }

  function revealPortfolio(animateLetters = true) {
    const ctx = gsap.context(() => {
      // Reveal main content
      gsap.to(content, {
        opacity: 1,
        scale: 1,
        filter: "blur(0px)",
        duration: 1.2,
        ease: "power3.out"
      });

      // Hero text reveal (letter by letter only if coming from intro)
      if (heroText) {
        if (animateLetters) {
          // Letter-by-letter animation when coming from intro
      gsap.from(heroText.children, {
        y: 80,
        opacity: 0,
        stagger: 0.08,
        duration: 1,
        ease: "power3.out"
          });
        } else {
          // Simple fade-in when skipping intro
          gsap.from(heroText, {
            opacity: 0,
            y: 30,
            duration: 0.8,
            ease: "power2.out"
          });
        }
      }

      // Animate sections on scroll
      gsap.utils.toArray("section").forEach((section: any) => {
        gsap.from(section, {
          scrollTrigger: {
            trigger: section,
            start: "top 85%",
            toggleActions: "play none none none"
          },
          y: 60,
          opacity: 0,
          duration: 0.8,
          ease: "power2.out"
        });
      });

      // Animate project cards
      gsap.utils.toArray(".project-card").forEach((card: any, i: number) => {
        gsap.from(card, {
        scrollTrigger: {
            trigger: card,
            start: "top 85%",
            toggleActions: "play none none none"
        },
          y: 40,
        opacity: 0,
          duration: 0.6,
          delay: i * 0.1,
          ease: "power2.out"
        });
      });
    });

    return () => ctx.revert();
  }
</script>

<!-- INTRO / LOADER -->
<div class="intro" bind:this={intro} on:click={startPortfolio} on:keydown={(e) => e.key === 'Enter' && startPortfolio()} role="button" tabindex="0" aria-label="Tap to proceed to portfolio">
  <div class="intro-background">
    <div class="gradient-orb orb-1"></div>
    <div class="gradient-orb orb-2"></div>
    <div class="gradient-orb orb-3"></div>
  </div>
  
  <div class="loader">
    <div class="loader-content">
      <div class="logo-text" bind:this={logoLetters} class:all-letters-ready={showTapPrompt}>
        <span class="logo-letter">L</span>
        <span class="logo-letter">A</span>
        <span class="logo-letter">W</span>
        <span class="logo-letter">R</span>
        <span class="logo-letter">E</span>
        <span class="logo-letter">N</span>
        <span class="logo-letter">C</span>
        <span class="logo-letter">E</span>
      </div>
      {#if showTapPrompt}
        <div class="tap-prompt">
          <p class="tap-text">Tap to proceed</p>
        </div>
    {/if}
    </div>
  </div>
</div>

<!-- PORTFOLIO -->
<div class="content" bind:this={content}>
  <!-- Navigation -->
  <nav class="nav">
    <div class="nav-content">
      <a href="#home" class="logo" on:click={closeMobileMenu}>Portfolio</a>
      <button class="mobile-menu-toggle" class:active={mobileMenuOpen} on:click={toggleMobileMenu} aria-label="Toggle menu">
        <span class="hamburger-line"></span>
        <span class="hamburger-line"></span>
        <span class="hamburger-line"></span>
      </button>
      <div class="nav-links" class:active={mobileMenuOpen}>
        <a href="#about" on:click={closeMobileMenu}>About</a>
        <a href="#skills" on:click={closeMobileMenu}>Skills</a>
        <a href="#projects" on:click={closeMobileMenu}>Projects</a>
        <a href="#contact" on:click={closeMobileMenu}>Contact</a>
      </div>
    </div>
    {#if mobileMenuOpen}
      <div class="mobile-menu-backdrop" class:active={mobileMenuOpen} on:click={closeMobileMenu} on:keydown={(e) => e.key === 'Enter' && closeMobileMenu()} role="button" tabindex="0" aria-label="Close menu"></div>
    {/if}
  </nav>

  <!-- Hero Section -->
  <section id="home" class="hero-section">
    <div class="hero-content">
      <div class="hero-left">
  <h1 class="hero" bind:this={heroText}>
          <span>H</span><span>i</span><span>,</span><span>&nbsp;</span>
          <span>I</span><span>'</span><span>m</span><span>&nbsp;</span>
          <span class="highlight">J</span><span class="highlight">o</span><span class="highlight">h</span><span class="highlight">n</span>
  </h1>
        <p class="hero-subtitle">Full Stack Developer & UI/UX Designer</p>
        <p class="hero-description">
          I create beautiful, functional, and user-centered digital experiences.
          Passionate about clean code and innovative solutions.
        </p>
        <div class="hero-buttons">
          <a href="#projects" class="btn btn-primary">View My Work</a>
          <a href="#contact" class="btn btn-secondary">Get In Touch</a>
        </div>
      </div>
      <div class="hero-right">
        <div 
          class="hero-visual" 
          bind:this={heroVisualContainer}
          role="img"
          aria-label="3D interactive hero visual"
        >
          <div class="hero-visual-inner" bind:this={heroVisualInner}>
            <div class="hero-image-wrapper">
              <img src={heroImage} alt="" class="hero-image" />
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="scroll-indicator">
      <div class="mouse"></div>
    </div>
  </section>

  <!-- About Section -->
  <About />

  <!-- Skills Section -->
  <Skills />

  <!-- Projects Section -->
  <Projects />

  <!-- Contact Section -->
  <Contact />

  <!-- Footer -->
  <footer class="footer">
    <div class="container">
      <p>&copy; 2024 Portfolio. Built with SvelteKit & GSAP</p>
    </div>
  </footer>
</div>

<style>
  :global(*) {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  :global(html) {
    scroll-behavior: smooth;
  }

  :global(body) {
    font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'SF Pro Text', 'Helvetica Neue', Helvetica, Arial, sans-serif;
    background: #000000;
    color: #f5f5f7;
    line-height: 1.47059;
    overflow-x: hidden;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  /* INTRO */
  .intro {
    position: fixed;
    inset: 0;
    background: #000000;
    color: #f5f5f7;
    display: grid;
    place-items: center;
    z-index: 100;
    cursor: pointer;
    overflow: hidden;
    width: 100%;
    height: 100%;
    box-sizing: border-box;
  }

  .intro-background {
    position: absolute;
    inset: 0;
    overflow: hidden;
    background: 
      radial-gradient(circle at 20% 30%, rgba(102, 126, 234, 0.15) 0%, transparent 50%),
      radial-gradient(circle at 80% 70%, rgba(118, 75, 162, 0.15) 0%, transparent 50%),
      radial-gradient(circle at 50% 50%, rgba(102, 126, 234, 0.1) 0%, transparent 70%),
      linear-gradient(135deg, #000000 0%, #0a0a0a 100%);
  }

  .intro-background::before {
    content: '';
    position: absolute;
    inset: 0;
    background-image: 
      linear-gradient(rgba(102, 126, 234, 0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(102, 126, 234, 0.03) 1px, transparent 1px);
    background-size: 50px 50px;
    animation: gridMove 20s linear infinite;
    opacity: 0.5;
  }

  .intro-background::after {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(ellipse at center, transparent 0%, rgba(0, 0, 0, 0.8) 100%);
  }

  @keyframes gridMove {
    0% {
      transform: translate(0, 0);
    }
    100% {
      transform: translate(50px, 50px);
    }
  }

  .gradient-orb {
    position: absolute;
    border-radius: 50%;
    filter: blur(100px);
    opacity: 0.3;
    animation: float 25s infinite ease-in-out;
  }

  .orb-1 {
    width: 500px;
    height: 500px;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    top: -250px;
    left: -250px;
    animation-delay: 0s;
  }

  .orb-2 {
    width: 400px;
    height: 400px;
    background: linear-gradient(135deg, #764ba2 0%, #667eea 100%);
    bottom: -200px;
    right: -200px;
    animation-delay: -8s;
  }

  .orb-3 {
    width: 450px;
    height: 450px;
    background: linear-gradient(135deg, rgba(102, 126, 234, 0.6) 0%, rgba(118, 75, 162, 0.6) 100%);
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    animation-delay: -16s;
  }

  @keyframes float {
    0%, 100% {
      transform: translate(0, 0) scale(1) rotate(0deg);
    }
    33% {
      transform: translate(50px, -50px) scale(1.2) rotate(120deg);
    }
    66% {
      transform: translate(-30px, 30px) scale(0.8) rotate(240deg);
    }
  }

  .loader {
    position: relative;
    z-index: 10;
    width: 100%;
    max-width: 500px;
    padding: 2rem;
    box-sizing: border-box;
    margin: 0 auto;
  }

  .loader-content {
    position: relative;
    text-align: center;
    width: 100%;
    overflow: visible;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .logo-text {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: nowrap;
    gap: 0.3rem;
    margin-bottom: 0;
    font-size: clamp(2.5rem, 6vw, 4rem);
    font-weight: 700;
    letter-spacing: 0.1em;
    width: 100%;
    max-width: 100%;
    box-sizing: border-box;
    padding: 0 2rem;
    line-height: 1.2;
    white-space: nowrap;
    position: relative;
    z-index: 1;
  }

  .logo-letter {
    display: inline-block;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    font-weight: 600;
    opacity: 0;
    transform: translateY(30px);
    flex-shrink: 0;
    filter: drop-shadow(0 0 8px rgba(102, 126, 234, 0.3));
    transition: filter 0.3s ease;
  }

  /* Apply floating and glowing animation only after ALL letters are loaded and tap prompt is shown */
  .logo-text.all-letters-ready .logo-letter.letter-loaded {
    animation: floatGlow 4s cubic-bezier(0.4, 0, 0.6, 1) infinite;
  }

  @keyframes floatGlow {
    0% {
      transform: translateY(0);
      filter: drop-shadow(0 0 8px rgba(102, 126, 234, 0.3));
    }
    25% {
      transform: translateY(-6px);
      filter: drop-shadow(0 0 12px rgba(102, 126, 234, 0.5)) drop-shadow(0 0 20px rgba(118, 75, 162, 0.3));
    }
    50% {
      transform: translateY(-8px);
      filter: drop-shadow(0 0 16px rgba(102, 126, 234, 0.6)) drop-shadow(0 0 24px rgba(118, 75, 162, 0.4));
    }
    75% {
      transform: translateY(-6px);
      filter: drop-shadow(0 0 12px rgba(102, 126, 234, 0.5)) drop-shadow(0 0 20px rgba(118, 75, 162, 0.3));
    }
    100% {
      transform: translateY(0);
      filter: drop-shadow(0 0 8px rgba(102, 126, 234, 0.3));
    }
  }

  /* Stagger the animation for each letter to create a smooth wave effect */
  .logo-text.all-letters-ready .logo-letter.letter-loaded:nth-child(1) { animation-delay: 0s; }
  .logo-text.all-letters-ready .logo-letter.letter-loaded:nth-child(2) { animation-delay: 0.2s; }
  .logo-text.all-letters-ready .logo-letter.letter-loaded:nth-child(3) { animation-delay: 0.4s; }
  .logo-text.all-letters-ready .logo-letter.letter-loaded:nth-child(4) { animation-delay: 0.6s; }
  .logo-text.all-letters-ready .logo-letter.letter-loaded:nth-child(5) { animation-delay: 0.8s; }
  .logo-text.all-letters-ready .logo-letter.letter-loaded:nth-child(6) { animation-delay: 1s; }
  .logo-text.all-letters-ready .logo-letter.letter-loaded:nth-child(7) { animation-delay: 1.2s; }
  .logo-text.all-letters-ready .logo-letter.letter-loaded:nth-child(8) { animation-delay: 1.4s; }



  .tap-prompt {
    position: absolute;
    top: 100%;
    margin-top: 3rem;
    left: 50%;
    transform: translateX(-50%);
    width: 100%;
    animation: fadeInUp 0.6s ease-out;
    z-index: 2;
  }

  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateX(-50%) translateY(20px);
    }
    to {
      opacity: 1;
      transform: translateX(-50%) translateY(0);
    }
  }

  .tap-text {
    font-size: 1.1rem;
    font-weight: 400;
    color: rgba(245, 245, 247, 0.8);
    letter-spacing: 0.15em;
    text-transform: uppercase;
    animation: pulse 2s infinite ease-in-out;
    margin: 0;
  }

  @keyframes pulse {
    0%, 100% {
      opacity: 0.6;
      transform: scale(1);
    }
    50% {
      opacity: 1;
      transform: scale(1.05);
    }
  }

  /* Intro Responsive */
  @media (max-width: 768px) {
    .loader {
      padding: 1.5rem;
      max-width: 95%;
    }

    .logo-text {
      font-size: clamp(2rem, 7vw, 3.5rem);
      gap: 0.2rem;
      padding: 0 1rem;
    }

    .tap-prompt {
      margin-top: 2rem;
    }

    .tap-text {
      font-size: 0.9rem;
    }

    .intro-background::before {
      background-size: 30px 30px;
    }

    .orb-1,
    .orb-2,
    .orb-3 {
      width: 300px;
      height: 300px;
    }
  }

  @media (max-width: 480px) {
    .loader {
      padding: 1rem;
      max-width: 100%;
    }

    .logo-text {
      font-size: clamp(1.5rem, 8vw, 2.5rem);
      gap: 0.15rem;
      padding: 0 0.5rem;
    }

    .tap-text {
      font-size: 0.85rem;
    }

    .orb-1,
    .orb-2,
    .orb-3 {
      width: 250px;
      height: 250px;
    }
  }

  /* CONTENT */
  .content {
    min-height: 100vh;
    background: #000000;
  }

  /* Navigation */
  .nav {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 50;
    background: rgba(0, 0, 0, 0.72);
    backdrop-filter: saturate(180%) blur(20px);
    -webkit-backdrop-filter: saturate(180%) blur(20px);
    border-bottom: 0.5px solid rgba(255, 255, 255, 0.1);
    padding: 0;
  }

  .nav-content {
    max-width: 1024px;
    margin: 0 auto;
    padding: 0 0.75rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 44px;
  }

  .logo {
    font-size: 1.1rem;
    font-weight: 600;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    text-decoration: none;
    letter-spacing: -0.01em;
    transition: opacity 0.3s;
  }

  .logo:hover {
    opacity: 0.8;
  }

  .nav-links {
    display: flex;
    gap: 0;
  }

  .nav-links a {
    color: rgba(245, 245, 247, 0.8);
    text-decoration: none;
    font-size: 0.85rem;
    font-weight: 400;
    padding: 0 0.75rem;
    transition: color 0.3s;
    position: relative;
    letter-spacing: -0.01em;
  }

  .nav-links a:hover {
    color: #667eea;
  }

  .mobile-menu-toggle {
    display: none;
    flex-direction: column;
    gap: 5px;
    background: transparent;
    border: none;
    cursor: pointer;
    padding: 0.5rem;
    z-index: 100;
    position: relative;
  }

  .hamburger-line {
    width: 25px;
    height: 2px;
    background: #e0e0e0;
    transition: all 0.3s;
    border-radius: 2px;
  }

  .mobile-menu-toggle.active .hamburger-line:nth-child(1) {
    transform: rotate(45deg) translate(8px, 8px);
  }

  .mobile-menu-toggle.active .hamburger-line:nth-child(2) {
    opacity: 0;
  }

  .mobile-menu-toggle.active .hamburger-line:nth-child(3) {
    transform: rotate(-45deg) translate(7px, -7px);
  }

  .mobile-menu-backdrop {
    display: none;
  }

  @media (max-width: 768px) {
    .mobile-menu-backdrop {
      display: block;
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.6);
      z-index: 98;
      backdrop-filter: blur(4px);
      -webkit-backdrop-filter: blur(4px);
      opacity: 0;
      transition: opacity 0.3s;
    }

    .mobile-menu-backdrop.active {
      opacity: 1;
    }
  }

  /* Hero Section */
  .hero-section {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 8rem 0 6rem;
    position: relative;
  }

  .hero-content {
    max-width: 1024px;
    margin: 0 auto;
    padding: 0 0.75rem;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 3rem;
    align-items: center;
  }

  .hero-left {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }

  .hero {
    font-size: clamp(3.5rem, 8vw, 6rem);
    font-weight: 600;
    display: flex;
    gap: 0.05em;
    overflow: hidden;
    margin-bottom: 0;
    line-height: 1.05;
    letter-spacing: -0.02em;
  }

  .hero span {
    display: inline-block;
  }

  .hero .highlight {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .hero-subtitle {
    font-size: clamp(1.4rem, 3vw, 2rem);
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 0;
    font-weight: 500;
    letter-spacing: 0.01em;
  }

  .hero-description {
    font-size: clamp(1.1rem, 2vw, 1.3rem);
    color: rgba(245, 245, 247, 0.7);
    margin-bottom: 0;
    line-height: 1.47059;
    font-weight: 400;
  }

  .hero-buttons {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
    margin-top: 1rem;
  }

  .hero-right {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .hero-visual {
    width: 100%;
    max-width: 400px;
    height: 400px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    cursor: pointer;
    perspective: 1000px;
  }

  .hero-visual-inner {
    width: 100%;
    height: 100%;
    position: relative;
    transform-style: preserve-3d;
    will-change: transform;
    backface-visibility: hidden;
    -webkit-backface-visibility: hidden;
    transform: translate3d(0, 0, 0);
  }



  .hero-image-wrapper {
    position: relative;
    z-index: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
  }

  .hero-image {
    width: 100%;
    max-width: 400px;
    height: auto;
    object-fit: contain;
    filter: drop-shadow(0 10px 30px rgba(102, 126, 234, 0.5));
    transform: translateZ(30px);
    will-change: transform;
    backface-visibility: hidden;
    -webkit-backface-visibility: hidden;
  }

  .btn {
    padding: 0.75rem 1.4rem;
    border-radius: 22px;
    text-decoration: none;
    font-weight: 400;
    font-size: 1.05rem;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    display: inline-block;
    border: 1px solid transparent;
    letter-spacing: -0.01em;
  }

  .btn-primary {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: #ffffff;
    border-color: transparent;
  }

  .btn-primary:hover {
    background: linear-gradient(135deg, #764ba2 0%, #667eea 100%);
    transform: scale(1.02);
  }

  .btn-secondary {
    background: transparent;
    color: #667eea;
    border-color: rgba(102, 126, 234, 0.4);
  }

  .btn-secondary:hover {
    background: rgba(102, 126, 234, 0.1);
    border-color: #667eea;
  }

  .scroll-indicator {
    position: absolute;
    bottom: 2rem;
    left: 50%;
    transform: translateX(-50%);
  }

  .mouse {
    width: 24px;
    height: 40px;
    border: 2px solid rgba(255, 255, 255, 0.3);
    border-radius: 12px;
    position: relative;
  }

  .mouse::before {
    content: '';
    position: absolute;
    top: 8px;
    left: 50%;
    transform: translateX(-50%);
    width: 4px;
    height: 8px;
    background: rgba(255, 255, 255, 0.5);
    border-radius: 2px;
    animation: scroll 1.5s infinite;
  }

  @keyframes scroll {
    0% { opacity: 1; transform: translateX(-50%) translateY(0); }
    100% { opacity: 0; transform: translateX(-50%) translateY(12px); }
  }

  /* Container */
  .container {
    max-width: 1024px;
    margin: 0 auto;
    padding: 0 0.75rem;
  }


  /* Footer */
  .footer {
    padding: 3.5rem 0;
    text-align: center;
    color: rgba(245, 245, 247, 0.4);
    border-top: 0.5px solid rgba(255, 255, 255, 0.1);
    font-size: 0.85rem;
    font-weight: 400;
  }

  .footer .container {
    padding: 0 0.75rem;
  }

  @media (max-width: 768px) {
    .footer {
      padding: 1.5rem 0;
    }

    .footer .container {
      padding: 0 1rem;
    }

    .footer p {
      font-size: 0.9rem;
    }
  }

  /* Responsive */
  @media (max-width: 768px) {
    .mobile-menu-toggle {
      display: flex;
    }

    .nav-content {
      padding: 0 1rem;
    }

    .nav-links {
      position: fixed;
      top: 0;
      right: -100%;
      width: 280px;
      height: 100vh;
      background: rgba(0, 0, 0, 0.95);
      backdrop-filter: saturate(180%) blur(20px);
      -webkit-backdrop-filter: saturate(180%) blur(20px);
      flex-direction: column;
      padding: 5rem 2rem 2rem;
      gap: 0;
      transition: right 0.3s cubic-bezier(0.4, 0, 0.2, 1);
      border-left: 0.5px solid rgba(255, 255, 255, 0.1);
      z-index: 99;
    }

    .nav-links.active {
      right: 0;
    }

    .nav-links a {
      font-size: 1.1rem;
      padding: 1rem 0;
      border-bottom: 0.5px solid rgba(255, 255, 255, 0.05);
      font-weight: 400;
      letter-spacing: -0.01em;
    }

    .nav-links a::after {
      display: none;
    }

    .hero-section {
      padding: 6rem 0 3rem;
    }

    .hero-content {
      grid-template-columns: 1fr;
      gap: 3rem;
      text-align: center;
    }

    .hero-left {
      order: 2;
    }

    .hero-right {
      order: 1;
    }

    .hero-visual {
      max-width: 300px;
      height: 300px;
    }

    .hero-image {
      max-width: 300px;
    }

    .hero {
      font-size: clamp(2.5rem, 10vw, 4rem);
      justify-content: center;
    }

    .hero-subtitle {
      font-size: clamp(1rem, 4vw, 1.4rem);
    }

    .hero-description {
      font-size: clamp(0.9rem, 3vw, 1.1rem);
    }

    .hero-buttons {
      flex-direction: column;
      align-items: stretch;
      width: 100%;
      max-width: 300px;
      margin: 0 auto;
    }

    .btn {
      text-align: center;
      width: 100%;
    }

    .container {
      padding: 0 1rem;
    }

    .scroll-indicator {
      display: none;
    }
  }

  @media (max-width: 480px) {
    .nav-content {
      padding: 0 1rem;
    }

    .logo {
      font-size: 1rem;
    }

    .hero-section {
      padding: 5rem 1rem 2rem;
    }

    .hero-content {
      gap: 2rem;
      padding: 0 1rem;
    }

    .hero {
      font-size: clamp(2rem, 12vw, 3rem);
    }

    .hero-visual {
      max-width: 250px;
      height: 250px;
    }

    .hero-image {
      max-width: 250px;
    }

    .btn {
      padding: 0.75rem 1.5rem;
      font-size: 0.9rem;
    }

    .container {
      padding: 0 1rem;
    }
  }
</style>
