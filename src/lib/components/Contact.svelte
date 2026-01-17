<script lang="ts">
  let name = "";
  let email = "";
  let message = "";
  let submitted = false;

  function handleSubmit(e: Event) {
    e.preventDefault();
    // Handle form submission here
    submitted = true;
    setTimeout(() => {
      submitted = false;
      name = "";
      email = "";
      message = "";
    }, 3000);
  }

  const contactInfo = [
    {
      title: "Email",
      value: "hello@example.com",
      link: "mailto:hello@example.com"
    },
    {
      title: "LinkedIn",
      value: "linkedin.com/in/profile",
      link: "https://linkedin.com"
    },
    {
      title: "GitHub",
      value: "github.com/username",
      link: "https://github.com"
    }
  ];
</script>

<section id="contact" class="contact-section">
  <div class="container">
    <div class="section-header">
      <h2 class="section-title">Get In Touch</h2>
    </div>

    <p class="section-description">
      I'm always open to discussing new projects, creative ideas, or opportunities to be part of your visions.
    </p>

    <div class="contact-content">
      <div class="contact-left">
        <div class="contact-items">
          {#each contactInfo as item}
            <a href={item.link} class="contact-item" target={item.link.startsWith('http') ? '_blank' : undefined}>
              <div class="contact-icon-wrapper">
                <svg class="contact-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                  {#if item.title === "Email"}
                    <path d="M4 4H20C21.1 4 22 4.9 22 6V18C22 19.1 21.1 20 20 20H4C2.9 20 2 19.1 2 18V6C2 4.9 2.9 4 4 4Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="L22 6L12 13L2 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                  {:else if item.title === "LinkedIn"}
                    <path d="M16 8C17.5913 8 19.1174 8.63214 20.2426 9.75736C21.3679 10.8826 22 12.4087 22 14V21H18V14C18 13.4696 17.7893 12.9609 17.4142 12.5858C17.0391 12.2107 16.5304 12 16 12C15.4696 12 14.9609 12.2107 14.5858 12.5858C14.2107 12.9609 14 13.4696 14 14V21H10V14C10 12.4087 10.6321 10.8826 11.7574 9.75736C12.8826 8.63214 14.4087 8 16 8Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M6 9H2V21H6V9Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M4 6C5.10457 6 6 5.10457 6 4C6 2.89543 5.10457 2 4 2C2.89543 2 2 2.89543 2 4C2 5.10457 2.89543 6 4 6Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                  {:else}
                    <path d="M9 19C4 20.5 4 16.5 2 16M22 16C20 16.5 20 20.5 15 19M22 13V16C22 17.5913 21.3679 19.1174 20.2426 20.2426C19.1174 21.3679 17.5913 22 16 22H8C6.4087 22 4.88258 21.3679 3.75736 20.2426C2.63214 19.1174 2 17.5913 2 16V13C2 11.4087 2.63214 9.88258 3.75736 8.75736C4.88258 7.63214 6.4087 7 8 7H16C17.5913 7 19.1174 7.63214 20.2426 8.75736C21.3679 9.88258 22 11.4087 22 13Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M12 15C13.6569 15 15 13.6569 15 12C15 10.3431 13.6569 9 12 9C10.3431 9 9 10.3431 9 12C9 13.6569 10.3431 15 12 15Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                  {/if}
                </svg>
              </div>
              <div class="contact-details">
                <h4 class="contact-item-title">{item.title}</h4>
                <span class="contact-item-value">{item.value}</span>
              </div>
              <svg class="contact-arrow" width="16" height="16" viewBox="0 0 16 16" fill="none">
                <path d="M6 12L10 8L6 4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </a>
          {/each}
        </div>
      </div>

      <div class="contact-right">
        <form class="contact-form" on:submit={handleSubmit}>
          <div class="form-group">
            <label for="name">Name</label>
            <input
              type="text"
              id="name"
              bind:value={name}
              placeholder="Your name"
              required
            />
          </div>

          <div class="form-group">
            <label for="email">Email</label>
            <input
              type="email"
              id="email"
              bind:value={email}
              placeholder="your.email@example.com"
              required
            />
          </div>

          <div class="form-group">
            <label for="message">Message</label>
            <textarea
              id="message"
              bind:value={message}
              placeholder="Tell me about your project or idea..."
              rows="6"
              required
            ></textarea>
          </div>

          <button type="submit" class="submit-btn" disabled={submitted}>
            {#if submitted}
              <span class="submit-success">Message Sent</span>
            {:else}
              <span>Send Message</span>
            {/if}
          </button>
        </form>
      </div>
    </div>
  </div>
</section>

<style>
  .contact-section {
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
    margin-bottom: 1.5rem;
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

  .section-description {
    color: rgba(245, 245, 247, 0.6);
    font-size: 1.4rem;
    margin-bottom: 5rem;
    max-width: 680px;
    line-height: 1.47059;
    font-weight: 400;
  }

  .contact-content {
    display: grid;
    grid-template-columns: 1fr 1.2fr;
    gap: 4rem;
    align-items: start;
  }

  .contact-items {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
  }

  .contact-item {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding: 1.5rem;
    background: rgba(245, 245, 247, 0.03);
    border: 0.5px solid rgba(102, 126, 234, 0.2);
    border-radius: 18px;
    text-decoration: none;
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
  }

  .contact-item:hover {
    background: rgba(245, 245, 247, 0.05);
    border-color: rgba(102, 126, 234, 0.4);
    transform: translateX(4px);
  }

  .contact-icon-wrapper {
    flex-shrink: 0;
  }

  .contact-icon {
    width: 24px;
    height: 24px;
    stroke: #667eea;
    fill: none;
    transition: all 0.3s;
  }

  .contact-item:hover .contact-icon {
    stroke: #764ba2;
    transform: scale(1.1);
  }

  .contact-details {
    flex: 1;
  }

  .contact-item-title {
    font-size: 0.85rem;
    color: rgba(245, 245, 247, 0.6);
    margin: 0 0 0.25rem 0;
    font-weight: 400;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .contact-item-value {
    font-size: 1.1rem;
    color: #f5f5f7;
    font-weight: 400;
    letter-spacing: -0.01em;
  }

  .contact-arrow {
    color: rgba(102, 126, 234, 0.4);
    transition: all 0.3s;
    flex-shrink: 0;
  }

  .contact-item:hover .contact-arrow {
    color: #667eea;
    transform: translateX(4px);
  }

  .contact-form {
    background: rgba(245, 245, 247, 0.03);
    border: 0.5px solid rgba(102, 126, 234, 0.2);
    border-radius: 18px;
    padding: 2.5rem;
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
  }

  .form-group {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .form-group label {
    color: rgba(245, 245, 247, 0.8);
    font-size: 0.9rem;
    font-weight: 400;
    letter-spacing: -0.01em;
  }

  .form-group input,
  .form-group textarea {
    width: 100%;
    padding: 1rem;
    background: rgba(245, 245, 247, 0.05);
    border: 0.5px solid rgba(102, 126, 234, 0.2);
    border-radius: 12px;
    color: #f5f5f7;
    font-family: inherit;
    font-size: 1.05rem;
    transition: all 0.3s;
    font-weight: 400;
    letter-spacing: -0.01em;
  }

  .form-group input:focus,
  .form-group textarea:focus {
    outline: none;
    border-color: #667eea;
    background: rgba(245, 245, 247, 0.08);
  }

  .form-group input::placeholder,
  .form-group textarea::placeholder {
    color: rgba(245, 245, 247, 0.4);
  }

  .form-group textarea {
    resize: vertical;
    min-height: 120px;
    font-family: inherit;
  }

  .submit-btn {
    margin-top: 0.5rem;
    padding: 1rem 2rem;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: #ffffff;
    border: 1px solid transparent;
    border-radius: 22px;
    font-size: 1.05rem;
    font-weight: 400;
    cursor: pointer;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    letter-spacing: -0.01em;
  }

  .submit-btn:hover:not(:disabled) {
    background: linear-gradient(135deg, #764ba2 0%, #667eea 100%);
    transform: scale(1.02);
  }

  .submit-btn:disabled {
    opacity: 0.7;
    cursor: not-allowed;
  }

  .submit-success {
    color: #ffffff;
    font-weight: 400;
  }

  @media (max-width: 768px) {
    .contact-section {
      padding: 4rem 0;
    }

    .contact-content {
      grid-template-columns: 1fr;
      gap: 3rem;
    }

    .section-header {
      margin-bottom: 1rem;
    }

    .section-title {
      font-size: 2.5rem;
    }

    .section-description {
      font-size: 1.2rem;
      margin-bottom: 3rem;
    }

    .contact-form {
      padding: 2rem;
    }
  }

  @media (max-width: 480px) {
    .contact-section {
      padding: 3rem 0;
    }

    .container {
      padding: 0 1rem;
    }

    .section-title {
      font-size: 2rem;
    }

    .section-description {
      font-size: 1.1rem;
      margin-bottom: 2.5rem;
    }

    .contact-form {
      padding: 1.5rem;
    }

    .contact-item {
      padding: 1.25rem;
      gap: 0.75rem;
    }

    .contact-icon {
      width: 20px;
      height: 20px;
    }

    .contact-item-title {
      font-size: 0.8rem;
    }

    .contact-item-value {
      font-size: 1rem;
    }

    .submit-btn {
      padding: 0.875rem 1.5rem;
      font-size: 1rem;
    }
  }
</style>
