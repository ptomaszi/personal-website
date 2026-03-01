<script setup lang="ts">
interface Experience {
  role: string
  company: string
  period?: string
  highlights?: string[]
}

const experiences: Experience[] = [
  {
    role: 'Lead AI Engineer',
    company: 'EcoOnline',
    period: '2024 – Present',
    highlights: [
      'Architect enterprise LLM ecosystems, evolving standard implementations into production-scale agentic development',
      'Design autonomous agent frameworks capable of complex reasoning and tool integration',
      'Lead a team of Data & AI engineers, fostering technical excellence and collaborative innovation',
      'Accelerate development with AI-powered tools — Claude Code, GitHub Copilot, and Amazon Kiro for agentic coding workflows',
    ],
  },
  {
    role: 'Lead Software Engineer',
    company: 'EcoOnline',
    period: '2020 – 2024',
    highlights: [
      'Built enterprise systems in .NET/Core, C#, across Azure and AWS',
      'Drove platform architecture decisions and established engineering standards',
      'Bridged full-stack engineering and cutting-edge AI — transforming raw model capabilities into stable, scalable applications',
    ],
  },
  {
    role: 'Lead Developer',
    company: 'Alcumus',
  },
  {
    role: 'Senior Software Engineer',
    company: 'Data Interchange',
  },
]
</script>

<template>
  <section id="experience" class="experience">
    <div class="container">
      <h2 class="section-title fade-in">Experience</h2>

      <div class="timeline">
        <div v-for="exp in experiences" :key="`${exp.company}-${exp.role}`" class="timeline-entry fade-in">
          <h3 class="entry-role">{{ exp.role }}</h3>
          <span class="entry-company">{{ exp.company }}</span>
          <span v-if="exp.period" class="entry-period">{{ exp.period }}</span>
          <ul v-if="exp.highlights?.length" class="entry-highlights">
            <li v-for="(item, i) in exp.highlights" :key="i">{{ item }}</li>
          </ul>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.experience {
  padding: var(--space-xl) 0;
  background: var(--bg-primary);
}

.timeline {
  position: relative;
  padding-left: 2rem;
  margin-top: var(--space-md);
}

.timeline::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 2px;
  background: linear-gradient(to bottom, var(--accent), rgba(0, 212, 255, 0.1));
}

.timeline-entry {
  position: relative;
  padding-bottom: var(--space-lg);
}

.timeline-entry:last-child {
  padding-bottom: 0;
}

.timeline-entry::before {
  content: '';
  position: absolute;
  left: calc(-2rem + -5px);
  top: 6px;
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: var(--bg-primary);
  border: 2px solid var(--accent);
  box-shadow: 0 0 10px var(--accent-glow);
}

.entry-role {
  font-size: 1.25rem;
  font-weight: 700;
  color: var(--text-bright);
  margin-bottom: 0.25rem;
}

.entry-company {
  font-family: var(--font-heading);
  font-size: 0.95rem;
  color: var(--accent);
  margin-right: var(--space-sm);
}

.entry-period {
  font-family: var(--font-heading);
  font-size: 0.85rem;
  color: var(--text-muted);
}

.entry-highlights {
  margin-top: var(--space-sm);
  padding-left: 1.2rem;
  list-style: none;
}

.entry-highlights li {
  position: relative;
  padding-left: 0.25rem;
  margin-bottom: 0.5rem;
  color: var(--text);
  font-size: 0.95rem;
  line-height: 1.6;
}

.entry-highlights li::before {
  content: '▹';
  position: absolute;
  left: -1.2rem;
  color: var(--accent);
}

@media (max-width: 480px) {
  .entry-role {
    font-size: 1.1rem;
  }

  .entry-company,
  .entry-period {
    display: block;
  }
}
</style>
