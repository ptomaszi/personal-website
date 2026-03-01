<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const canvasRef = ref<HTMLCanvasElement | null>(null)
let animationId = 0

interface Particle {
  x: number
  y: number
  vx: number
  vy: number
  r: number
}

function initParticles() {
  const canvas = canvasRef.value
  if (!canvas) return

  const ctx = canvas.getContext('2d')
  if (!ctx) return

  const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  if (prefersReducedMotion) return

  let particles: Particle[] = []
  const CONNECTION_DIST = 150
  const PARTICLE_COUNT_FACTOR = 8000

  function resize() {
    canvas!.width = canvas!.offsetWidth
    canvas!.height = canvas!.offsetHeight
    initParticleArray()
  }

  function initParticleArray() {
    const count = Math.floor((canvas!.width * canvas!.height) / PARTICLE_COUNT_FACTOR)
    particles = Array.from({ length: Math.min(count, 120) }, () => ({
      x: Math.random() * canvas!.width,
      y: Math.random() * canvas!.height,
      vx: (Math.random() - 0.5) * 0.5,
      vy: (Math.random() - 0.5) * 0.5,
      r: Math.random() * 1.5 + 0.5,
    }))
  }

  function draw() {
    ctx!.clearRect(0, 0, canvas!.width, canvas!.height)

    for (let i = 0; i < particles.length; i++) {
      const p = particles[i]
      p.x += p.vx
      p.y += p.vy

      if (p.x < 0 || p.x > canvas!.width) p.vx *= -1
      if (p.y < 0 || p.y > canvas!.height) p.vy *= -1

      ctx!.beginPath()
      ctx!.arc(p.x, p.y, p.r, 0, Math.PI * 2)
      ctx!.fillStyle = 'rgba(0, 212, 255, 0.5)'
      ctx!.fill()

      for (let j = i + 1; j < particles.length; j++) {
        const q = particles[j]
        const dx = p.x - q.x
        const dy = p.y - q.y
        const dist = Math.sqrt(dx * dx + dy * dy)

        if (dist < CONNECTION_DIST) {
          ctx!.beginPath()
          ctx!.moveTo(p.x, p.y)
          ctx!.lineTo(q.x, q.y)
          ctx!.strokeStyle = `rgba(0, 212, 255, ${0.15 * (1 - dist / CONNECTION_DIST)})`
          ctx!.lineWidth = 0.5
          ctx!.stroke()
        }
      }
    }

    animationId = requestAnimationFrame(draw)
  }

  resize()
  window.addEventListener('resize', resize)
  draw()

  onUnmounted(() => {
    cancelAnimationFrame(animationId)
    window.removeEventListener('resize', resize)
  })
}

onMounted(initParticles)
</script>

<template>
  <section id="about" class="hero">
    <canvas ref="canvasRef" class="hero-canvas" aria-hidden="true"></canvas>
    <div class="hero-content container">
      <div class="hero-text">
        <p class="hero-greeting">Hello, I'm</p>
        <h1 class="hero-name">Tomasz Polecki</h1>
        <h2 class="hero-title">Lead AI Engineer</h2>
        <p class="hero-tagline">
          Architecting intelligent systems at the intersection of engineering and AI
        </p>
        <div class="hero-badge">
          <span class="badge-icon">⚡</span>
          Building with Claude Code · Copilot · Kiro
        </div>
        <div class="hero-cta">
          <a href="https://github.com/ptomaszi" target="_blank" rel="noopener noreferrer" class="btn btn-primary" aria-label="GitHub profile">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor"><path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z"/></svg>
            GitHub
          </a>
          <a href="https://linkedin.com/in/tomaszpolecki" target="_blank" rel="noopener noreferrer" class="btn btn-outline" aria-label="LinkedIn profile">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 01-2.063-2.065 2.064 2.064 0 112.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
            LinkedIn
          </a>
          <a href="mailto:tomasz.polecki@ecoonline.com" class="btn btn-outline" aria-label="Send email">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor"><path d="M20 4H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z"/></svg>
            Email
          </a>
        </div>
      </div>
    </div>

    <div class="about container">
      <div class="about-content fade-in">
        <p>
          As Lead AI Engineer at <strong>EcoOnline</strong>, I architect the technical foundation for
          enterprise LLM ecosystems — evolving standard implementations into production-scale agentic
          development. I design autonomous agent frameworks capable of complex reasoning and tool
          integration, ensuring LLM workloads remain performant, cost-efficient, and scalable.
        </p>
        <p>
          With 4+ years as a Lead Software Engineer building enterprise systems in .NET, Azure, and AWS,
          I bridge full-stack engineering and cutting-edge AI — transforming raw model capabilities into
          stable, scalable applications.
        </p>
      </div>
    </div>
  </section>
</template>

<style scoped>
.hero {
  position: relative;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding-top: var(--nav-height);
}

.hero-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}

.hero-content {
  position: relative;
  z-index: 1;
  flex: 1;
  display: flex;
  align-items: center;
  min-height: calc(100vh - var(--nav-height));
}

.hero-text {
  max-width: 700px;
}

.hero-greeting {
  font-family: var(--font-heading);
  font-size: 1rem;
  color: var(--accent);
  letter-spacing: 0.1em;
  margin-bottom: var(--space-xs);
}

.hero-name {
  font-size: clamp(2.5rem, 6vw, 4.5rem);
  font-weight: 700;
  margin-bottom: var(--space-xs);
  background: linear-gradient(135deg, var(--text-bright), var(--accent));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-title {
  font-size: clamp(1.2rem, 3vw, 1.8rem);
  font-weight: 400;
  color: var(--text-muted);
  margin-bottom: var(--space-sm);
}

.hero-tagline {
  font-size: 1.1rem;
  color: var(--text);
  max-width: 500px;
  margin-bottom: var(--space-md);
  line-height: 1.7;
}

.hero-badge {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  font-family: var(--font-heading);
  font-size: 0.85rem;
  color: var(--accent);
  background: var(--accent-dim);
  border: 1px solid rgba(0, 212, 255, 0.2);
  border-radius: 100px;
  padding: 0.5rem 1.2rem;
  margin-bottom: var(--space-md);
  letter-spacing: 0.03em;
}

.badge-icon {
  font-size: 1rem;
}

.hero-cta {
  display: flex;
  gap: var(--space-sm);
  flex-wrap: wrap;
}

.btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-family: var(--font-heading);
  font-size: 0.875rem;
  padding: 0.7rem 1.4rem;
  border-radius: var(--radius);
  letter-spacing: 0.03em;
  transition: all var(--transition);
  cursor: pointer;
  border: 1px solid transparent;
}

.btn-primary {
  background: var(--accent);
  color: var(--bg-primary);
  border-color: var(--accent);
}

.btn-primary:hover {
  background: transparent;
  color: var(--accent);
  box-shadow: 0 0 20px var(--accent-glow);
}

.btn-outline {
  background: transparent;
  color: var(--accent);
  border-color: var(--accent);
}

.btn-outline:hover {
  background: var(--accent-dim);
  box-shadow: 0 0 20px var(--accent-glow);
  color: var(--text-bright);
}

.about {
  position: relative;
  z-index: 1;
  padding: var(--space-xl) 0;
}

.about-content {
  max-width: 700px;
  display: flex;
  flex-direction: column;
  gap: var(--space-sm);
  border-left: 3px solid var(--accent);
  padding-left: var(--space-md);
}

.about-content p {
  font-size: 1.05rem;
  line-height: 1.8;
  color: var(--text);
}

.about-content strong {
  color: var(--accent);
}

@media (max-width: 768px) {
  .hero-content {
    min-height: calc(80vh - var(--nav-height));
  }

  .hero-cta {
    flex-direction: column;
  }

  .btn {
    justify-content: center;
  }
}
</style>
