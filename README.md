<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Time Inc. - Premium time for premium people" />
    <title>Time Inc.™ - Premium Time Trading Platform</title>

    <style>
      :root {
        --primary: #6a11cb;
        --secondary: #2575fc;
        --accent: #ff6a00;
        --danger: #ee0979;
        --white: #ffffff;
        --dark: #111111;
        --gold: #ffdd57;
        --text-primary: rgba(255, 255, 255, 1);
        --text-secondary: rgba(255, 255, 255, 0.9);
        --bg-light: rgba(255, 255, 255, 0.1);
        --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        --shadow-lg: 0 25px 50px rgba(0, 0, 0, 0.4);
      }

      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }

      html {
        scroll-behavior: smooth;
      }

      body {
        min-height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        overflow-x: hidden;
        background: linear-gradient(-45deg, var(--primary), var(--secondary), var(--accent), var(--danger));
        background-size: 400% 400%;
        animation: gradientMove 10s ease infinite;
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Poppins', sans-serif;
        color: var(--text-primary);
        padding: 20px;
      }

      @media (prefers-reduced-motion: reduce) {
        * {
          animation-duration: 0.01ms !important;
          animation-iteration-count: 1 !important;
          transition-duration: 0.01ms !important;
        }
      }

      @keyframes gradientMove {
        0% {
          background-position: 0% 50%;
        }
        50% {
          background-position: 100% 50%;
        }
        100% {
          background-position: 0% 50%;
        }
      }

      @keyframes float {
        0% {
          transform: translateY(0);
        }
        50% {
          transform: translateY(-15px);
        }
        100% {
          transform: translateY(0);
        }
      }

      @keyframes rise {
        from {
          transform: translateY(100vh) translateX(0);
          opacity: 1;
        }
        to {
          transform: translateY(-10vh) translateX(100px);
          opacity: 0;
        }
      }

      .container {
        background: var(--bg-light);
        backdrop-filter: blur(15px);
        -webkit-backdrop-filter: blur(15px);
        padding: clamp(30px, 5vw, 50px);
        border-radius: 25px;
        text-align: center;
        width: 100%;
        max-width: 400px;
        box-shadow: var(--shadow-lg);
        animation: float 4s ease-in-out infinite;
        border: 1px solid rgba(255, 255, 255, 0.2);
      }

      h1 {
        font-size: clamp(24px, 5vw, 32px);
        margin-bottom: 10px;
        font-weight: 700;
        letter-spacing: 0.5px;
      }

      .tagline {
        font-size: 14px;
        color: var(--text-secondary);
        margin-bottom: 20px;
        min-height: 20px;
        line-height: 1.5;
      }

      .clock {
        font-size: clamp(24px, 4vw, 32px);
        margin: 20px 0;
        font-weight: 700;
        letter-spacing: 2px;
        font-variant-numeric: tabular-nums;
        font-family: 'Courier New', monospace;
      }

      .price {
        background: var(--white);
        color: var(--dark);
        padding: 12px 16px;
        border-radius: 15px;
        font-weight: 600;
        margin-bottom: 15px;
        font-size: 15px;
      }

      .stock {
        font-size: 13px;
        margin-bottom: 20px;
        color: var(--gold);
        font-weight: 500;
      }

      button {
        padding: clamp(10px, 2vw, 12px) clamp(20px, 3vw, 25px);
        border: 2px solid transparent;
        border-radius: 30px;
        background: var(--white);
        color: var(--dark);
        font-weight: 600;
        font-size: 14px;
        cursor: pointer;
        transition: var(--transition);
        box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
      }

      button:hover {
        transform: scale(1.1) translateY(-2px);
        background: var(--gold);
        box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
      }

      button:active {
        transform: scale(0.98);
      }

      button:focus-visible {
        outline: 2px solid var(--gold);
        outline-offset: 2px;
      }

      .message {
        margin-top: 20px;
        font-size: 14px;
        min-height: 20px;
        animation: slideIn 0.5s ease;
      }

      @keyframes slideIn {
        from {
          opacity: 0;
          transform: translateY(-10px);
        }
        to {
          opacity: 1;
          transform: translateY(0);
        }
      }

      .particle {
        position: fixed;
        width: 6px;
        height: 6px;
        background: var(--white);
        border-radius: 50%;
        pointer-events: none;
        animation: rise 8s linear infinite;
      }

      .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0, 0, 0, 0);
        white-space: nowrap;
        border-width: 0;
      }

      @media (max-width: 480px) {
        .container {
          border-radius: 20px;
        }

        h1 {
          margin-bottom: 8px;
        }
      }
    </style>
  </head>
  <body>
    <div class="container">
      <h1>⏳ Time Inc.</h1>
      <p class="tagline" id="typing" aria-label="Tagline"></p>

      <time class="clock" id="clock" aria-live="polite" aria-label="Current time"></time>

      <div class="price">💸 Price: Free hai enjoy karo</div>
      <div class="stock" id="stock" aria-live="polite" aria-label="Stock availability"></div>

      <button id="buyBtn" aria-label="Claim unlimited time offer">Claim Unlimited Time</button>

      <div class="message" id="msg" aria-live="assertive" aria-label="Status message"></div>
    </div>

    <script>
      // Configuration
      const CONFIG = {
        typingSpeed: 40,
        stockUpdateInterval: 3000,
        particleCount: 25,
        tagline: 'We sell premium time to premium people.',
      };

      // Clock Update
      function updateClock() {
        const now = new Date();
        const timeString = now.toLocaleTimeString('en-US', {
          hour: '2-digit',
          minute: '2-digit',
          second: '2-digit',
          hour12: true,
        });
        document.getElementById('clock').textContent = timeString;
      }

      // Initialize clock
      updateClock();
      setInterval(updateClock, 1000);

      // Stock Counter
      let stock = 7;
      const stockEl = document.getElementById('stock');

      function updateStock() {
        if (stock > 1) {
          stock--;
          stockEl.textContent = `⚠️ Only ${stock} minutes left in stock!`;
        } else {
          stockEl.textContent = '😱 Time SOLD OUT... just kidding it\'s unlimited.';
        }
      }

      stockEl.textContent = `⚠️ Only ${stock} minutes left in stock!`;
      setInterval(updateStock, CONFIG.stockUpdateInterval);

      // Typing Effect
      let charIndex = 0;

      function typeNextChar() {
        if (charIndex < CONFIG.tagline.length) {
          const typingElement = document.getElementById('typing');
          typingElement.textContent += CONFIG.tagline[charIndex];
          charIndex++;
          setTimeout(typeNextChar, CONFIG.typingSpeed);
        }
      }

      typeNextChar();

      // Buy Button Handler
      document.getElementById('buyBtn').addEventListener('click', function () {
        const msgEl = document.getElementById('msg');
        msgEl.textContent =
          '🎉 Congratulations! You now own 24 hours daily. Use wisely. Or waste creatively.';
        
        // Add visual feedback
        this.disabled = true;
        setTimeout(() => {
          this.disabled = false;
        }, 2000);
      });

      // Floating Particles
      function createParticles() {
        const container = document.body;
        for (let i = 0; i < CONFIG.particleCount; i++) {
          const particle = document.createElement('div');
          particle.className = 'particle';
          particle.style.left = Math.random() * 100 + 'vw';
          particle.style.top = '100vh';
          particle.style.animationDuration = 5 + Math.random() * 5 + 's';
          particle.style.animationDelay = Math.random() * 2 + 's';
          particle.style.opacity = Math.random() * 0.7 + 0.3;
          container.appendChild(particle);
        }
      }

      createParticles();

      // Prevent memory leaks by removing old particles
      setInterval(() => {
        const particles = document.querySelectorAll('.particle');
        if (particles.length > CONFIG.particleCount * 2) {
          particles.forEach((particle, index) => {
            if (index % 2 === 0) particle.remove();
          });
        }
      }, 30000);
    </script>
  </body>
</html>
