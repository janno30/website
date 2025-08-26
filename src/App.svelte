<script>
  import { Github, MessageCircle, Heart } from 'lucide-svelte';
  import { onMount } from 'svelte';
  
  let canvas;
  let ctx;
  let animationId;
  let planets = [];
  let particles = [];
  
  function handleGitHub() {
    window.open('https://github.com/janno30', '_blank');
  }
  
  function handleDiscord() {
    window.open('https://discord.com/users/953028229357244486', '_blank');
  }
  
  function handleDonate() {
    // You can replace this with your actual donation link
    window.open('https://ko-fi.com/janno', '_blank');
  }
  
  // Planet class for physics simulation
  class Planet {
    constructor(x, y, radius, color, velocityX = 0, velocityY = 0) {
      this.x = x;
      this.y = y;
      this.radius = radius;
      this.color = color;
      this.velocityX = velocityX;
      this.velocityY = velocityY;
      this.gravity = 0.1;
      this.friction = 0.99;
    }
    
    update() {
      this.x += this.velocityX;
      this.y += this.velocityY;
      
      // Apply gravity
      this.velocityY += this.gravity;
      
      // Apply friction
      this.velocityX *= this.friction;
      this.velocityY *= this.friction;
      
      // Bounce off walls
      if (this.x - this.radius <= 0 || this.x + this.radius >= canvas.width) {
        this.velocityX *= -0.8;
        this.x = Math.max(this.radius, Math.min(canvas.width - this.radius, this.x));
      }
      
      if (this.y - this.radius <= 0 || this.y + this.radius >= canvas.height) {
        this.velocityY *= -0.8;
        this.y = Math.max(this.radius, Math.min(canvas.height - this.radius, this.y));
      }
    }
    
    draw() {
      ctx.beginPath();
      ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
      ctx.fillStyle = this.color;
      ctx.fill();
      
      // Add glow effect
      ctx.shadowColor = this.color;
      ctx.shadowBlur = 20;
      ctx.fill();
      ctx.shadowBlur = 0;
    }
  }
  
  // Particle class for background effects
  class Particle {
    constructor(x, y) {
      this.x = x;
      this.y = y;
      this.velocityX = (Math.random() - 0.5) * 2;
      this.velocityY = (Math.random() - 0.5) * 2;
      this.life = 1;
      this.decay = 0.02;
      this.size = Math.random() * 3 + 1;
    }
    
    update() {
      this.x += this.velocityX;
      this.y += this.velocityY;
      this.life -= this.decay;
    }
    
    draw() {
      ctx.save();
      ctx.globalAlpha = this.life;
      ctx.fillStyle = '#8b5cf6';
      ctx.beginPath();
      ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
      ctx.fill();
      ctx.restore();
    }
  }
  
  function initCanvas() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    
    // Create planets
    planets = [
      new Planet(100, 100, 30, '#8b5cf6', 1, 0.5),
      new Planet(300, 200, 25, '#ec4899', -0.8, 0.3),
      new Planet(500, 150, 35, '#06b6d4', 0.5, -0.7),
      new Planet(200, 400, 20, '#10b981', -0.3, 0.9),
      new Planet(600, 350, 28, '#f59e0b', 0.7, -0.4)
    ];
  }
  
  function animate() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    
    // Update and draw planets
    planets.forEach(planet => {
      planet.update();
      planet.draw();
    });
    
    // Update and draw particles
    particles = particles.filter(particle => particle.life > 0);
    particles.forEach(particle => {
      particle.update();
      particle.draw();
    });
    
    // Add new particles randomly
    if (Math.random() < 0.1) {
      particles.push(new Particle(
        Math.random() * canvas.width,
        Math.random() * canvas.height
      ));
    }
    
    // Add particles from planet collisions
    planets.forEach(planet => {
      if (Math.random() < 0.05) {
        particles.push(new Particle(planet.x, planet.y));
      }
    });
    
    animationId = requestAnimationFrame(animate);
  }
  
  function handleResize() {
    if (canvas) {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }
  }
  
  onMount(() => {
    if (canvas) {
      ctx = canvas.getContext('2d');
      initCanvas();
      animate();
      
      window.addEventListener('resize', handleResize);
      
      return () => {
        window.removeEventListener('resize', handleResize);
        if (animationId) {
          cancelAnimationFrame(animationId);
        }
      };
    }
  });
</script>

<canvas bind:this={canvas} class="background-canvas"></canvas>

<main>
  <div class="container">
    <div class="content">
      <!-- Left section with illustration -->
      <div class="illustration-section">
        <div class="illustration">
          <div class="character">
            <div class="head">
              <div class="hair"></div>
              <div class="face">
                <div class="eyes">
                  <div class="eye left">
                    <div class="iris"></div>
                  </div>
                  <div class="eye right">
                    <div class="iris"></div>
                  </div>
                </div>
                <div class="dark-circles">
                  <div class="circle left"></div>
                  <div class="circle right"></div>
                </div>
                <div class="mouth"></div>
              </div>
            </div>
            <div class="body">
              <div class="shirt">
                <div class="collar"></div>
                <div class="shirt-buttons"></div>
              </div>
              <div class="tie"></div>
              <div class="hand"></div>
            </div>
          </div>
        </div>
      </div>

      <!-- Right section with text and buttons -->
      <div class="text-section">
        <h1 class="greeting">hey, i'm janno</h1>
        <p class="description">
          Hi! Just someone who likes coding and stuff. I'm from the Philippines :)
        </p>
        
        <div class="buttons">
          <button class="btn github" on:click={handleGitHub}>
            <Github size={20} />
            <span>GitHub</span>
          </button>
          <button class="btn discord" on:click={handleDiscord}>
            <MessageCircle size={20} />
            <span>Discord</span>
          </button>
          <button class="btn donate" on:click={handleDonate}>
            <Heart size={20} />
            <span>Donate</span>
          </button>
        </div>

        <div class="social-card">
          <div class="avatar">
            <div class="avatar-face">
              <div class="avatar-hair"></div>
              <div class="avatar-eyes">
                <div class="avatar-eye left"></div>
                <div class="avatar-eye right"></div>
              </div>
            </div>
          </div>
          <div class="social-info">
            <div class="name">Janno</div>
            <div class="handle">@janno30</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</main>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .background-canvas {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
  }

  :global(body) {
    font-family: 'Inter', sans-serif;
    background-color: transparent;
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow-x: hidden;
  }

  main {
    width: 100%;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px;
    position: relative;
    z-index: 1;
  }

  .container {
    background-color: rgba(74, 50, 50, 0.95);
    backdrop-filter: blur(10px);
    border-radius: 20px;
    padding: 40px;
    max-width: 900px;
    width: 100%;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
    border: 1px solid rgba(255, 255, 255, 0.1);
  }

  .content {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 40px;
    align-items: center;
  }

  /* Illustration Section */
  .illustration-section {
    display: flex;
    justify-content: center;
  }

  .illustration {
    width: 300px;
    height: 300px;
    background-color: rgba(92, 48, 92, 0.9);
    border-radius: 20px;
    position: relative;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    backdrop-filter: blur(5px);
    border: 1px solid rgba(255, 255, 255, 0.1);
  }

  .character {
    position: relative;
    width: 200px;
    height: 200px;
  }

  .head {
    position: relative;
    width: 120px;
    height: 120px;
    margin: 0 auto;
  }

  .hair {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 80px;
    background-color: #1a1a1a;
    border-radius: 60px 60px 40px 40px;
    transform: rotate(-5deg);
  }

  .face {
    position: absolute;
    top: 20px;
    left: 10px;
    width: 100px;
    height: 100px;
    background-color: #f4d3c3;
    border-radius: 50px;
  }

  .eyes {
    position: absolute;
    top: 35px;
    left: 20px;
    display: flex;
    gap: 25px;
  }

  .eye {
    width: 20px;
    height: 20px;
    background-color: white;
    border-radius: 50%;
    position: relative;
  }

  .iris {
    position: absolute;
    top: 2px;
    left: 2px;
    width: 16px;
    height: 16px;
    background-color: #8b5cf6;
    border-radius: 50%;
  }

  .dark-circles {
    position: absolute;
    top: 55px;
    left: 20px;
    display: flex;
    gap: 25px;
  }

  .circle {
    width: 20px;
    height: 8px;
    background-color: #6b7280;
    border-radius: 4px;
  }

  .mouth {
    position: absolute;
    bottom: 25px;
    left: 35px;
    width: 30px;
    height: 15px;
    border: 2px solid #8b5a2b;
    border-top: none;
    border-radius: 0 0 30px 30px;
  }

  .body {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 80px;
    height: 100px;
  }

  .shirt {
    position: relative;
    width: 80px;
    height: 80px;
    background-color: white;
    border-radius: 40px 40px 20px 20px;
  }

  .collar {
    position: absolute;
    top: 10px;
    left: 20px;
    width: 40px;
    height: 20px;
    background-color: #f8f9fa;
    border-radius: 10px;
  }

  .shirt-buttons {
    position: absolute;
    top: 35px;
    left: 25px;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .shirt-buttons::before,
  .shirt-buttons::after {
    content: '';
    width: 8px;
    height: 8px;
    background-color: #dee2e6;
    border-radius: 50%;
  }

  .tie {
    position: absolute;
    top: 15px;
    left: 30px;
    width: 20px;
    height: 50px;
    background-color: #1e293b;
    border-radius: 10px;
  }

  .hand {
    position: absolute;
    top: 25px;
    right: -10px;
    width: 25px;
    height: 40px;
    background-color: #f4d3c3;
    border-radius: 12px;
    transform: rotate(15deg);
  }

  /* Text Section */
  .text-section {
    color: #E0D9D9;
  }

  .greeting {
    font-size: 2.5rem;
    font-weight: 600;
    margin-bottom: 20px;
    line-height: 1.2;
  }

  .description {
    font-size: 1.1rem;
    line-height: 1.6;
    margin-bottom: 30px;
    opacity: 0.9;
  }

  .buttons {
    display: flex;
    gap: 15px;
    margin-bottom: 25px;
  }

  .btn {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 12px 20px;
    background-color: rgba(224, 192, 192, 0.9);
    color: #2C2020;
    border: none;
    border-radius: 12px;
    font-size: 0.95rem;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s ease;
    flex: 1;
    justify-content: center;
    backdrop-filter: blur(5px);
  }

  .btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
    background-color: rgba(224, 192, 192, 1);
  }

  .social-card {
    display: flex;
    align-items: center;
    gap: 15px;
    padding: 15px 20px;
    background-color: rgba(224, 192, 192, 0.9);
    border-radius: 12px;
    color: #2C2020;
    backdrop-filter: blur(5px);
  }

  .avatar {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background-color: rgba(92, 48, 92, 0.9);
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
  }

  .avatar-face {
    position: relative;
    width: 35px;
    height: 35px;
  }

  .avatar-hair {
    position: absolute;
    top: 0;
    left: 0;
    width: 35px;
    height: 20px;
    background-color: #1a1a1a;
    border-radius: 17px 17px 10px 10px;
  }

  .avatar-eyes {
    position: absolute;
    top: 12px;
    left: 8px;
    display: flex;
    gap: 8px;
  }

  .avatar-eye {
    width: 6px;
    height: 6px;
    background-color: #8b5cf6;
    border-radius: 50%;
  }

  .social-info {
    flex: 1;
  }

  .name {
    font-weight: 600;
    font-size: 1.1rem;
    margin-bottom: 2px;
  }

  .handle {
    font-size: 0.9rem;
    opacity: 0.8;
  }

  /* Responsive Design */
  @media (max-width: 768px) {
    .content {
      grid-template-columns: 1fr;
      gap: 30px;
    }

    .illustration {
      width: 250px;
      height: 250px;
    }

    .greeting {
      font-size: 2rem;
    }

    .buttons {
      flex-direction: column;
    }
  }
</style>
