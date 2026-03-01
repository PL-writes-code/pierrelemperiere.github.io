<script setup lang="ts">
import { useData, useRoute } from 'vitepress'
import { computed, ref, onMounted } from 'vue'
import { Content } from 'vitepress'

const { frontmatter, site } = useData()
const route = useRoute()

const themes = ['a', 'b', 'c', 'd'] as const
const themeNames: Record<string, string> = {
  a: 'Dark Terminal',
  b: 'Swiss Minimal',
  c: 'Document-First',
  d: 'Editorial Dark',
}

const currentTheme = ref<string>('a')
const switcherOpen = ref(false)

onMounted(() => {
  const saved = localStorage.getItem('blog-theme')
  if (saved && themes.includes(saved as any)) currentTheme.value = saved
})

function setTheme(t: string) {
  currentTheme.value = t
  localStorage.setItem('blog-theme', t)
}

function toggleSwitcher() {
  switcherOpen.value = !switcherOpen.value
}

const isHome = computed(() => route.path === '/' || route.path === '/index.html')
const isPosts = computed(() => route.path === '/posts' || route.path === '/posts/' || route.path === '/posts/index.html')
const isArticle = computed(() => !isHome.value && !isPosts.value)

const posts = [
  {
    title: 'Building an AI-Powered Home Server',
    url: '/posts/2026-03-01-building-an-ai-powered-home-server',
    date: '2026-03-01',
    description: 'How I set up a dedicated SER8 mini PC running Arch Linux as a persistent AI assistant.',
  },
]

function formatDate(dateStr: string | Date) {
  if (dateStr instanceof Date) {
    const y = dateStr.getFullYear()
    const m = dateStr.getMonth()
    const d = dateStr.getDate()
    const months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec']
    return `${months[m]} ${d}, ${y}`
  }
  const [y, m, d] = String(dateStr).split('-').map(Number)
  const months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec']
  return `${months[m - 1]} ${d}, ${y}`
}
</script>

<template>
  <div :class="'theme-' + currentTheme">

    <!-- ========== THEME SWITCHER ========== -->
    <div :class="['switcher', { open: switcherOpen }]" @click="!switcherOpen && toggleSwitcher()">
      <span class="switcher-chevron" @click.stop="toggleSwitcher">‹</span>
      <span class="switcher-label" @click.stop="toggleSwitcher">Theme</span>
      <button
        v-for="t in themes"
        :key="t"
        :class="['switcher-btn', { active: currentTheme === t }]"
        @click.stop="setTheme(t)"
      >
        {{ themeNames[t] }}
      </button>
    </div>

    <!-- ========== THEME A: Dark Terminal ========== -->
    <template v-if="currentTheme === 'a'">
      <nav class="a-nav">
        <a href="/" class="a-nav-logo">pierre<span class="a-cursor">_</span></a>
        <div class="a-nav-links">
          <a href="/" :class="{ active: isHome }">Home</a>
          <a href="/posts/" :class="{ active: isPosts || isArticle }">Posts</a>
        </div>
        <div class="a-nav-social">
          <a href="https://github.com/PL-writes-code" target="_blank"><svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor"><path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/></svg></a>
        </div>
      </nav>
      <div class="a-layout">
        <div v-if="isHome" class="a-home">
          <h1 class="a-home-name">Pierre Lemperiere</h1>
          <p class="a-home-tagline">Notes on systems, tools, and automation.</p>
          <a href="/posts/" class="a-home-cta">Read posts <span class="arrow">→</span></a>
        </div>
        <div v-else-if="isPosts">
          <div class="a-posts-header"><h1>Posts</h1></div>
          <ul class="a-post-list">
            <li v-for="post in posts" :key="post.url" class="a-post-item">
              <a :href="post.url">
                <span class="a-post-title">{{ post.title }}</span>
                <span class="a-post-date">{{ formatDate(post.date) }}</span>
              </a>
            </li>
          </ul>
        </div>
        <div v-else class="a-article">
          <a href="/posts/" class="a-back-link">← Posts</a>
          <div class="a-article-header">
            <div class="a-article-date" v-if="frontmatter.date">{{ formatDate(frontmatter.date) }}</div>
            <h1 class="a-article-title">{{ frontmatter.title }}</h1>
            <p class="a-article-desc" v-if="frontmatter.description">{{ frontmatter.description }}</p>
          </div>
          <div class="a-content"><Content /></div>
        </div>
        <footer class="a-footer"><div class="a-footer-inner"><span>Pierre Lemperiere</span><span>{{ new Date().getFullYear() }}</span></div></footer>
      </div>
    </template>

    <!-- ========== THEME B: Swiss Minimal ========== -->
    <template v-if="currentTheme === 'b'">
      <nav class="b-nav">
        <a href="/" class="b-nav-name">Pierre Lemperiere</a>
        <div class="b-nav-links"><a href="/posts/">Writing</a></div>
      </nav>
      <main class="b-main">
        <div v-if="isHome" class="b-home">
          <div class="b-circle" aria-hidden="true"></div>
          <div class="b-home-inner">
            <h1 class="b-home-name">Pierre<br>Lemperiere</h1>
            <p class="b-home-tagline">Notes on systems, tools, and automation.</p>
            <a href="/posts/" class="b-home-cta">Read the blog <span class="arrow">→</span></a>
          </div>
        </div>
        <div v-else-if="isPosts" class="b-posts-page">
          <h1 class="b-posts-heading">Writing</h1>
          <ul class="b-posts-list">
            <li v-for="post in posts" :key="post.url" class="b-post-item">
              <a :href="post.url"><span class="b-post-title">{{ post.title }}</span><span class="b-post-date">{{ formatDate(post.date) }}</span></a>
            </li>
          </ul>
        </div>
        <div v-else class="b-article">
          <div class="b-article-header">
            <p class="b-article-date" v-if="frontmatter.date">{{ formatDate(frontmatter.date) }}</p>
            <h1 class="b-article-title">{{ frontmatter.title }}</h1>
            <p class="b-article-desc" v-if="frontmatter.description">{{ frontmatter.description }}</p>
          </div>
          <div class="b-content"><Content /></div>
        </div>
      </main>
      <footer class="b-footer"><div class="b-footer-inner"><span>© {{ new Date().getFullYear() }} Pierre Lemperiere</span></div></footer>
    </template>

    <!-- ========== THEME C: Document-First ========== -->
    <template v-if="currentTheme === 'c'">
      <nav class="c-nav">
        <a href="/" class="c-nav-name">Pierre</a>
        <a href="/posts/" class="c-nav-link">Writing</a>
      </nav>
      <main class="c-main">
        <template v-if="isHome">
          <section class="c-letter">
            <p>I'm Pierre — a developer and tinkerer building things at the intersection of software, hardware, and curiosity. I care about simple tools, honest interfaces, and systems that stay out of your way.</p>
            <p>This site is where I think out loud about the things I'm making and learning.</p>
          </section>
          <section class="c-writing">
            <h2>Writing</h2>
            <ul>
              <li v-for="post in posts" :key="post.url">
                <a :href="post.url">{{ post.title }}</a>
                <span class="c-post-date">{{ post.date }}</span>
              </li>
            </ul>
          </section>
        </template>
        <template v-else-if="isPosts">
          <section class="c-writing">
            <h2>Writing</h2>
            <ul>
              <li v-for="post in posts" :key="post.url">
                <a :href="post.url">{{ post.title }}</a>
                <span class="c-post-date">{{ post.date }}</span>
              </li>
            </ul>
          </section>
        </template>
        <template v-else>
          <article class="c-article">
            <header>
              <h1 class="c-article-title">{{ frontmatter.title }}</h1>
              <time class="c-article-date" v-if="frontmatter.date">{{ formatDate(frontmatter.date) }}</time>
            </header>
            <div class="c-content"><Content /></div>
          </article>
        </template>
      </main>
      <footer class="c-footer"><span>Pierre</span></footer>
    </template>

    <!-- ========== THEME D: Editorial Dark ========== -->
    <template v-if="currentTheme === 'd'">
      <nav class="d-nav">
        <a href="/" class="d-nav-logo">Pierre Lemperiere</a>
        <div class="d-nav-links">
          <a href="/" :class="{ active: isHome }">Home</a>
          <a href="/posts/" :class="{ active: isPosts || isArticle }">Posts</a>
          <a href="https://github.com/PL-writes-code" target="_blank">GitHub</a>
        </div>
      </nav>
      <main class="d-main">
        <div v-if="isHome" class="d-home">
          <div class="d-glow" aria-hidden="true"></div>
          <h1 class="d-home-title">Thoughts on <span class="d-accent">Code</span>, Design &amp; Technology</h1>
          <p class="d-home-tagline">A personal notebook on software engineering, infrastructure, and the tools that shape how we build things.</p>
          <a href="/posts/" class="d-home-cta">Read the journal →</a>
        </div>
        <div v-else-if="isPosts" class="d-posts-page">
          <h1 class="d-posts-heading">Journal</h1>
          <div class="d-post-cards">
            <a v-for="post in posts" :key="post.url" :href="post.url" class="d-post-card">
              <span class="d-card-date" v-if="post.date">{{ formatDate(post.date) }}</span>
              <h2 class="d-card-title">{{ post.title }}</h2>
              <p class="d-card-desc" v-if="post.description">{{ post.description }}</p>
            </a>
          </div>
        </div>
        <div v-else class="d-article">
          <div class="d-article-header">
            <span class="d-article-date" v-if="frontmatter.date">{{ formatDate(frontmatter.date) }}</span>
            <h1 class="d-article-title">{{ frontmatter.title }}</h1>
            <p class="d-article-desc" v-if="frontmatter.description">{{ frontmatter.description }}</p>
          </div>
          <div class="d-content"><Content /></div>
        </div>
      </main>
      <footer class="d-footer"><div class="d-footer-inner"><span>© {{ new Date().getFullYear() }} · Built with VitePress</span></div></footer>
    </template>

  </div>
</template>
