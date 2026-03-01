<script setup lang="ts">
interface Skill {
  label: string
  icon: string
}

interface SkillCategory {
  title: string
  emoji: string
  skills: Skill[]
}

const categories: SkillCategory[] = [
  {
    title: 'AI / ML',
    emoji: '🧠',
    skills: [
      { label: 'AWS Bedrock', icon: '☁️' },
      { label: 'Strands Agents SDK', icon: '🤖' },
      { label: 'LiteLLM', icon: '⚡' },
      { label: 'LLM Orchestration', icon: '🔗' },
    ],
  },
  {
    title: 'Backend',
    emoji: '⚙️',
    skills: [
      { label: '.NET 9 / C#', icon: '🟣' },
      { label: 'Python', icon: '🐍' },
      { label: 'DynamoDB', icon: '🗄️' },
      { label: 'Kafka', icon: '📨' },
    ],
  },
  {
    title: 'Frontend',
    emoji: '🎨',
    skills: [
      { label: 'Vue 3', icon: '💚' },
      { label: 'TypeScript', icon: '🔷' },
      { label: 'Lit', icon: '🔥' },
      { label: 'Vite', icon: '⚡' },
    ],
  },
  {
    title: 'Infrastructure',
    emoji: '🏗️',
    skills: [
      { label: 'AWS', icon: '☁️' },
      { label: 'OpenTofu', icon: '🌱' },
      { label: 'Docker', icon: '🐳' },
      { label: 'CI/CD', icon: '🔄' },
    ],
  },
  {
    title: 'Leadership',
    emoji: '🚀',
    skills: [
      { label: 'Team Leadership', icon: '👥' },
      { label: 'Architecture', icon: '🏛️' },
      { label: 'Technical Strategy', icon: '🎯' },
      { label: 'Mentoring', icon: '📖' },
    ],
  },
]
</script>

<template>
  <section id="skills" class="skills">
    <div class="container">
      <h2 class="section-title fade-in">Skills & Tech Stack</h2>

      <div class="categories">
        <div v-for="category in categories" :key="category.title" class="category fade-in">
          <h3 class="category-title">
            <span class="category-emoji">{{ category.emoji }}</span>
            {{ category.title }}
          </h3>
          <div class="skill-grid">
            <div v-for="skill in category.skills" :key="skill.label" class="skill-card">
              <span class="skill-icon">{{ skill.icon }}</span>
              <span class="skill-label">{{ skill.label }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.skills {
  padding: var(--space-xl) 0;
  background: var(--bg-secondary);
}

.categories {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: var(--space-md);
}

.category {
  background: var(--bg-primary);
  border: 1px solid rgba(255, 255, 255, 0.05);
  border-radius: calc(var(--radius) * 2);
  padding: var(--space-md);
  transition: border-color var(--transition), box-shadow var(--transition);
}

.category:hover {
  border-color: rgba(0, 212, 255, 0.2);
  box-shadow: 0 4px 30px rgba(0, 212, 255, 0.05);
}

.category-title {
  font-size: 1.1rem;
  font-weight: 600;
  margin-bottom: var(--space-sm);
  display: flex;
  align-items: center;
  gap: var(--space-xs);
}

.category-emoji {
  font-size: 1.3rem;
}

.skill-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--space-xs);
}

.skill-card {
  display: flex;
  align-items: center;
  gap: var(--space-xs);
  padding: 0.6rem 0.8rem;
  background: var(--bg-secondary);
  border-radius: var(--radius);
  border: 1px solid transparent;
  transition: all var(--transition);
  cursor: default;
}

.skill-card:hover {
  border-color: var(--accent);
  box-shadow: 0 0 12px var(--accent-glow);
  transform: translateY(-2px);
}

.skill-icon {
  font-size: 1.1rem;
  flex-shrink: 0;
}

.skill-label {
  font-family: var(--font-heading);
  font-size: 0.8rem;
  color: var(--text);
  letter-spacing: 0.02em;
}

@media (max-width: 768px) {
  .categories {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 480px) {
  .skill-grid {
    grid-template-columns: 1fr;
  }
}
</style>
