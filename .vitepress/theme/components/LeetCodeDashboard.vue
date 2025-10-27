<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const topics = ref({})
const searchTerm = ref('')
const activeFilter = ref('all')
const expandedTopics = ref(new Set())
const sortBy = ref('default')
const showStats = ref(false)
const viewMode = ref('grid')

async function loadMarkdown() {
  try {
    const res = await fetch('/Prep/Leetcode/problems.md')
    if (!res.ok) {
      res = await fetch('/Leetcode/problems.md')
    }
    if (!res.ok) throw new Error(`fetch failed: ${res.status}`)
    const text = await res.text()
    parseMarkdown(text)
  } catch (e) {
    console.error(
      'Failed to load that markdown — make sure the file is at provided location',
      e,
    )
  }
}

function extractProblemNumber(link) {
  const match = link.match(/\/problems\/[^\/]+\/(\d+)/)
  if (match) return match[1]

  const match2 = link.match(/\/(\d+)[-_]/)
  if (match2) return match2[1]

  const match3 = link.match(/(\d{4})/)
  if (match3) return match3[1]

  return null
}

async function fetchDifficulty(problemIdentifier) {
  try {
    const match = problemIdentifier.match(/^(\d+)-/)
    if (!match) return 'medium' // fallback if format is wrong

    const problemNumber = parseInt(match[1], 10)

    // Fetch local problems.json
    const data = await fetch('/Prep/data/problems.json').then((r) => r.json())

    const problem = data[problemNumber] // now works even if input had leading zeros

    return problem?.difficulty || 'medium'
  } catch (err) {
    console.warn(`Could not fetch difficulty for ${problemIdentifier}:`, err)
    return 'medium'
  }
}

async function fetchAllProblemsData() {
  try {
    const res = await fetch('/Prep/data/problems.json')
    if (!res.ok) {
      res = await fetch('/data/problems.json')
    }
    return await res.json()
  } catch (err) {
    console.warn('Could not fetch problems.json:', err)
    return {}
  }
}

async function parseMarkdown(mdText) {
  const data = await fetchAllProblemsData()
  const sections = mdText.split(/^##\s+/m).slice(1)
  const result = {}

  for (const section of sections) {
    const lines = section.split('\n')
    const title = lines[0].trim()
    const problems = []

    for (const line of lines.slice(1)) {
      const m = line.match(/\[([^\]]+)\]\(([^)]+)\)/)
      if (!m) continue

      const name = m[1].trim()
      const link = m[2].trim()
      let difficulty = 'medium'
      const lname = name.toLowerCase()

      if (lname.includes('easy') || lname.match(/\beasy\b/i))
        difficulty = 'easy'
      else if (lname.includes('hard') || lname.match(/\bhard\b/i))
        difficulty = 'hard'
      else {
        const problemNum = extractProblemNumber(link)
        if (problemNum) {
          const numInt = parseInt(problemNum)

          const problemFromData = data[parseInt(problemNum, 10)]
          if (problemFromData?.difficulty)
            difficulty = problemFromData.difficulty
        }
      }
      problems.push({ name, link, difficulty })
    }
    result[title || 'Untitled'] = problems
  }
  topics.value = result
}

const toggleTopic = (topic) => {
  if (expandedTopics.value.has(topic)) {
    expandedTopics.value.delete(topic)
  } else {
    expandedTopics.value.add(topic)
  }
  expandedTopics.value = new Set(expandedTopics.value)
}

const isExpanded = (topic) => expandedTopics.value.has(topic)

const filteredProblems = (probs) => {
  return probs.filter((p) => {
    if (activeFilter.value !== 'all' && p.difficulty !== activeFilter.value)
      return false
    if (
      searchTerm.value &&
      !p.name.toLowerCase().includes(searchTerm.value.toLowerCase())
    )
      return false
    return true
  })
}

const allProblemsFlat = computed(() => {
  const all = []
  for (const [topic, problems] of Object.entries(topics.value)) {
    for (const p of problems) {
      all.push({ ...p, topic })
    }
  }
  return all
})

const searchResults = computed(() => {
  if (!searchTerm.value) return []

  return allProblemsFlat.value.filter((p) => {
    const matchesSearch = p.name
      .toLowerCase()
      .includes(searchTerm.value.toLowerCase())
    const matchesFilter =
      activeFilter.value === 'all' || p.difficulty === activeFilter.value
    return matchesSearch && matchesFilter
  })
})

const filteredTopics = computed(() => {
  if (searchTerm.value) return []

  let result = Object.entries(topics.value).filter(([_, problems]) => {
    return filteredProblems(problems).length > 0
  })

  const stripEmoji = (str) =>
    str
      .replace(/[\p{Emoji}\p{Extended_Pictographic}\uFE0F\u200D]+/gu, '')
      .trim()

  // Sort topics
  if (sortBy.value === 'name-asc') {
    result.sort((a, b) => stripEmoji(a[0]).localeCompare(stripEmoji(b[0])))
  } else if (sortBy.value === 'name-desc') {
    result.sort((a, b) => stripEmoji(b[0]).localeCompare(stripEmoji(a[0])))
  } else if (sortBy.value === 'count-asc') {
    result.sort(
      (a, b) => filteredProblems(a[1]).length - filteredProblems(b[1]).length,
    )
  } else if (sortBy.value === 'count-desc') {
    result.sort(
      (a, b) => filteredProblems(b[1]).length - filteredProblems(a[1]).length,
    )
  }
  return result
})

const totalCount = computed(() =>
  Object.values(topics.value).reduce((sum, arr) => sum + arr.length, 0),
)

const filterCounts = computed(() => {
  const counts = { all: 0, easy: 0, medium: 0, hard: 0 }
  for (const problems of Object.values(topics.value)) {
    for (const p of problems) {
      counts.all++
      counts[p.difficulty]++
    }
  }
  return counts
})

const progressStats = computed(() => {
  const total = filterCounts.value.all
  return {
    easy: {
      count: filterCounts.value.easy,
      percent: Math.round((filterCounts.value.easy / total) * 100),
    },
    medium: {
      count: filterCounts.value.medium,
      percent: Math.round((filterCounts.value.medium / total) * 100),
    },
    hard: {
      count: filterCounts.value.hard,
      percent: Math.round((filterCounts.value.hard / total) * 100),
    },
  }
})

const expandAll = () => {
  const allTopics = Object.keys(topics.value)
  expandedTopics.value = new Set(allTopics)
}

const collapseAll = () => {
  expandedTopics.value = new Set()
}

onMounted(loadMarkdown)
</script>

<template>
  <div class="lc-container">
    <header class="lc-header">
      <h1>🚀 LeetCode Solutions</h1>
      <div class="lc-badges">
        <span class="lc-badge">{{ totalCount }} Problems</span>
        <span class="lc-badge">Multiple Languages</span>
        <span
          class="lc-badge"
          @click="showStats = !showStats"
          style="cursor: pointer"
        >
          📊 Stats
        </span>
      </div>
    </header>

    <!-- Stats Dashboard -->
    <transition name="stats-fade">
      <div v-if="showStats" class="stats-dashboard">
        <div class="stats-grid">
          <div class="stat-card easy-stat">
            <div class="stat-icon">✅</div>
            <div class="stat-details">
              <div class="stat-number">{{ progressStats.easy.count }}</div>
              <div class="stat-label">Easy Problems</div>
            </div>
            <div class="stat-percent">{{ progressStats.easy.percent }}%</div>
          </div>

          <div class="stat-card medium-stat">
            <div class="stat-icon">⚡</div>
            <div class="stat-details">
              <div class="stat-number">{{ progressStats.medium.count }}</div>
              <div class="stat-label">Medium Problems</div>
            </div>
            <div class="stat-percent">{{ progressStats.medium.percent }}%</div>
          </div>

          <div class="stat-card hard-stat">
            <div class="stat-icon">🔥</div>
            <div class="stat-details">
              <div class="stat-number">{{ progressStats.hard.count }}</div>
              <div class="stat-label">Hard Problems</div>
            </div>
            <div class="stat-percent">{{ progressStats.hard.percent }}%</div>
          </div>

          <div class="stat-card total-stat">
            <div class="stat-icon">📚</div>
            <div class="stat-details">
              <div class="stat-number">{{ Object.keys(topics).length }}</div>
              <div class="stat-label">Total Topics</div>
            </div>
          </div>
        </div>
      </div>
    </transition>

    <div class="lc-controls">
      <input
        v-model="searchTerm"
        class="lc-search"
        placeholder="🔍 Search problems or topics..."
        aria-label="search problems"
      />

      <div class="control-group">
        <select v-model="sortBy" class="lc-sort">
          <option value="default">Default Order</option>
          <option value="name-asc">Name A-Z</option>
          <option value="name-desc">Name Z-A</option>
          <option value="count-asc">Least Problems</option>
          <option value="count-desc">Most Problems</option>
        </select>

        <button @click="expandAll" class="action-btn" title="Expand All">
          ⬇️
        </button>
        <button @click="collapseAll" class="action-btn" title="Collapse All">
          ⬆️
        </button>
      </div>
    </div>

    <div class="filter-bar">
      <div class="lc-filters" role="tablist" aria-label="difficulty filters">
        <button
          v-for="t in ['all', 'easy', 'medium', 'hard']"
          :key="t"
          :class="['lc-filter', { active: activeFilter === t }]"
          @click="activeFilter = t"
        >
          {{ t.charAt(0).toUpperCase() + t.slice(1) }}
          <span class="filter-count">({{ filterCounts[t] }})</span>
        </button>
      </div>
    </div>

    <!-- Search Results View -->
    <div v-if="searchTerm" class="search-results">
      <div class="search-header">
        <h2>Search Results</h2>
        <span class="result-count"
          >{{ searchResults.length }} problems found</span
        >
      </div>

      <div v-if="searchResults.length > 0" class="results-list">
        <div
          v-for="p in searchResults"
          :key="p.link"
          :class="['result-item', p.difficulty]"
        >
          <div class="result-content">
            <a
              :href="p.link"
              class="problem-name"
              rel="noopener"
              target="_blank"
            >
              {{ p.name }}
            </a>
            <span class="problem-topic">{{ p.topic }}</span>
          </div>
          <span class="difficulty" :class="p.difficulty">
            {{ p.difficulty.charAt(0).toUpperCase() + p.difficulty.slice(1) }}
          </span>
        </div>
      </div>

      <div v-else class="no-results">
        <div class="no-results-icon">🔍</div>
        <p>No problems found matching "{{ searchTerm }}"</p>
        <button @click="searchTerm = ''" class="clear-search">
          Clear Search
        </button>
      </div>
    </div>

    <!-- Topics Grid View -->
    <div v-else class="lc-grid">
      <section
        v-for="[topic, problems] in filteredTopics"
        :key="topic"
        class="topic-card"
      >
        <div
          class="topic-head"
          @click="toggleTopic(topic)"
          @keydown.enter="toggleTopic(topic)"
          @keydown.space.prevent="toggleTopic(topic)"
          role="button"
          tabindex="0"
          :aria-expanded="isExpanded(topic)"
        >
          <div class="topic-title">{{ topic }}</div>
          <div class="topic-meta">
            <span class="topic-count">{{
              filteredProblems(problems).length
            }}</span>
            <span class="topic-toggle">{{
              isExpanded(topic) ? '▲' : '▼'
            }}</span>
          </div>
        </div>

        <div class="problem-list" :class="{ open: isExpanded(topic) }">
          <div
            v-for="p in filteredProblems(problems)"
            :key="p.name"
            class="problem-item"
          >
            <a
              :href="p.link"
              class="problem-name"
              rel="noopener"
              target="_blank"
              >{{ p.name }}</a
            >

            <span class="difficulty" :class="p.difficulty">{{
              p.difficulty.charAt(0).toUpperCase() + p.difficulty.slice(1)
            }}</span>
          </div>

          <div v-if="filteredProblems(problems).length === 0" class="no-match">
            No problems match the current filter.
          </div>
        </div>
      </section>

      <div v-if="filteredTopics.length === 0" class="no-topics">
        <p>No topics match the current filter.</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.lc-container {
  max-width: 1600px;
  margin: 0 auto;
  padding: 2rem 3rem;
  color: var(--vp-c-text-1, #c9d1d9);
  font-family:
    -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue',
    Arial;
}

.lc-header {
  text-align: center;
  margin-bottom: 1.25rem;
}

.lc-header h1 {
  font-size: 2.25rem;
  margin-bottom: 0.5rem;
  background: linear-gradient(135deg, #ffa116, #f89b29);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.lc-badges {
  display: flex;
  gap: 0.75rem;
  justify-content: center;
  flex-wrap: wrap;
  margin-top: 0.5rem;
}

.lc-badge {
  background: var(--vp-c-bg-soft, #161b22);
  border: 1px solid var(--vp-c-divider, #30363d);
  padding: 0.35rem 0.8rem;
  border-radius: 16px;
  font-size: 0.9rem;
}

.lc-controls {
  display: flex;
  gap: 1rem;
  align-items: center;
  justify-content: space-between;
  margin: 1.25rem 0 0.75rem;
  flex-wrap: wrap;
}

.control-group {
  display: flex;
  gap: 0.5rem;
  align-items: center;
}

.lc-sort {
  padding: 0.6rem 0.9rem;
  background: var(--vp-c-bg-soft, #161b22);
  border: 1px solid var(--vp-c-divider, #30363d);
  border-radius: 8px;
  color: inherit;
  cursor: pointer;
  font-size: 0.95rem;
  transition: all 0.2s;
}

.lc-sort:hover {
  border-color: #ffa116;
}

.lc-sort:focus {
  outline: none;
  border-color: #ffa116;
}

.action-btn {
  padding: 0.6rem 0.9rem;
  background: var(--vp-c-bg-soft, #161b22);
  border: 1px solid var(--vp-c-divider, #30363d);
  border-radius: 8px;
  color: var(--vp-c-text-2, #8b949e);
  cursor: pointer;
  font-size: 1rem;
  transition: all 0.2s;
}

.action-btn:hover {
  border-color: #ffa116;
  color: #ffa116;
  background: rgba(255, 161, 22, 0.06);
  transform: translateY(-2px);
}

.lc-search {
  flex: 1;
  max-width: 500px;
  padding: 0.9rem 1rem;
  background: var(--vp-c-bg-soft, #161b22);
  border: 1px solid var(--vp-c-divider, #30363d);
  border-radius: 12px;
  color: inherit;
  font-size: 1rem;
}

.lc-search:focus {
  outline: none;
  border-color: #ffa116;
  box-shadow: 0 0 0 3px rgba(255, 161, 22, 0.1);
}

.filter-bar {
  display: flex;
  justify-content: center;
  margin: 0.75rem 0 1.5rem;
}

.lc-filters {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
}

.lc-filter {
  background: var(--vp-c-bg-soft, #161b22);
  border: 1px solid var(--vp-c-divider, #30363d);
  color: var(--vp-c-text-2, #8b949e);
  padding: 0.6rem 0.9rem;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
  font-size: 0.95rem;
}

.lc-filter:hover {
  border-color: #ffa116;
  color: #ffa116;
  background: rgba(255, 161, 22, 0.06);
}

.lc-filter.active {
  border-color: #ffa116;
  color: #ffa116;
  background: rgba(255, 161, 22, 0.08);
  font-weight: 600;
}

.filter-count {
  font-size: 0.85rem;
  opacity: 0.8;
  margin-left: 0.25rem;
}

.lc-grid {
  display: grid;
  gap: 1rem;
  grid-template-columns: 1fr;
  margin-top: 0.5rem;
  align-items: start;
}

@media (min-width: 1400px) {
  .lc-grid {
    grid-template-columns: 1fr !important;
  }
}

@media (max-width: 900px) {
  .lc-grid {
    grid-template-columns: 1fr;
  }
}

.topic-card {
  background: var(--vp-c-bg-soft, #161b22);
  border: 1px solid var(--vp-c-divider, #30363d);
  border-radius: 12px;
  padding: 0.5rem;
  transition:
    transform 0.18s,
    border-color 0.18s,
    box-shadow 0.18s;
}

.topic-card:hover {
  transform: translateY(-4px);
  border-color: #ffa116;
  box-shadow: 0 8px 16px rgba(255, 161, 22, 0.06);
}

.topic-head {
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  user-select: none;
  padding: 1rem;
  border-radius: 12px;
}

.topic-head:focus {
  outline: 2px solid #ffa116;
  outline-offset: 2px;
  border-radius: 4px;
}

.topic-title {
  font-weight: 600;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 1.05rem;
}

.topic-meta {
  display: flex;
  gap: 0.5rem;
  align-items: center;
}

.topic-count {
  background: #ffa116;
  color: #0d1117;
  padding: 0.25rem 0.6rem;
  border-radius: 10px;
  font-weight: 700;
  font-size: 0.85rem;
}

.topic-toggle {
  color: var(--vp-c-text-2, #8b949e);
  font-size: 0.9rem;
  transition: transform 0.2s;
}

.problem-list {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.3s ease;
  margin-top: 0;
}

.problem-list.open {
  max-height: 600px;
  overflow: auto;
  margin-top: 0.75rem;
  padding-right: 6px;
}

.problem-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: var(--vp-c-bg, #0d1117);
  border-left: 3px solid var(--vp-c-divider, #30363d);
  border-radius: 8px;
  padding: 0.6rem;
  margin: 0.5rem 0;
  transition: all 0.2s;
}

.problem-item:has(.difficulty.easy):hover {
  background: var(--vp-c-bg-alt, #1c2128);
  border-left-color: #00b8a3;
  transform: translateX(4px);
}

.problem-item:has(.difficulty.medium):hover {
  background: var(--vp-c-bg-alt, #1c2128);
  border-left-color: #ffa116;
  transform: translateX(4px);
}

.problem-item:has(.difficulty.hard):hover {
  background: var(--vp-c-bg-alt, #1c2128);
  border-left-color: #ef4444;
  transform: translateX(4px);
}

.problem-name {
  color: inherit;
  text-decoration: none;
  font-size: 0.92rem;
  flex: 1;
  margin-right: 0.5rem;
}

.problem-name:hover {
  color: #ffa116;
}

.difficulty {
  padding: 0.2rem 0.5rem;
  border-radius: 6px;
  font-weight: 700;
  font-size: 0.75rem;
  white-space: nowrap;
}

.difficulty.easy {
  background: rgba(0, 184, 163, 0.12);
  color: #00b8a3;
}

.difficulty.medium {
  background: rgba(255, 161, 22, 0.12);
  color: #ffa116;
}

.difficulty.hard {
  background: rgba(239, 68, 68, 0.12);
  color: #ef4444;
}

.no-match,
.no-topics {
  padding: 1rem;
  color: var(--vp-c-text-2, #8b949e);
  font-size: 0.9rem;
  text-align: center;
}

.problem-list.open::-webkit-scrollbar {
  width: 8px;
}

.problem-list.open::-webkit-scrollbar-thumb {
  background: var(--vp-c-divider, #30363d);
  border-radius: 6px;
}

/* Stats Dashboard */
.stats-dashboard {
  margin: 1.5rem 0;
  padding: 1.5rem;
  background: var(--vp-c-bg-soft, #161b22);
  border: 1px solid var(--vp-c-divider, #30363d);
  border-radius: 16px;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 1rem;
}

.stat-card {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1.25rem;
  background: var(--vp-c-bg, #0d1117);
  border-radius: 12px;
  border: 2px solid var(--vp-c-divider, #30363d);
  transition: all 0.3s;
}

.stat-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

.stat-card.easy-stat {
  border-color: rgba(0, 184, 163, 0.3);
}

.stat-card.easy-stat:hover {
  border-color: #00b8a3;
  box-shadow: 0 8px 16px rgba(0, 184, 163, 0.2);
}

.stat-card.medium-stat {
  border-color: rgba(255, 161, 22, 0.3);
}

.stat-card.medium-stat:hover {
  border-color: #ffa116;
  box-shadow: 0 8px 16px rgba(255, 161, 22, 0.2);
}

.stat-card.hard-stat {
  border-color: rgba(239, 68, 68, 0.3);
}

.stat-card.hard-stat:hover {
  border-color: #ef4444;
  box-shadow: 0 8px 16px rgba(239, 68, 68, 0.2);
}

.stat-card.total-stat {
  border-color: rgba(139, 148, 158, 0.3);
}

.stat-card.total-stat:hover {
  border-color: #8b949e;
}

.stat-icon {
  font-size: 2.5rem;
  line-height: 1;
}

.stat-details {
  flex: 1;
}

.stat-number {
  font-size: 2rem;
  font-weight: 700;
  line-height: 1;
  margin-bottom: 0.25rem;
}

.stat-label {
  font-size: 0.9rem;
  color: var(--vp-c-text-2, #8b949e);
}

.stat-percent {
  font-size: 1.5rem;
  font-weight: 700;
  opacity: 0.5;
}

.stats-fade-enter-active,
.stats-fade-leave-active {
  transition: all 0.3s ease;
}

.stats-fade-enter-from {
  opacity: 0;
  transform: translateY(-20px);
}

.stats-fade-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}

.search-results {
  margin-top: 1.5rem;
}

.search-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.75rem;
  border-bottom: 2px solid var(--vp-c-divider, #30363d);
}

.search-header h2 {
  font-size: 1.5rem;
  margin: 0;
  color: var(--vp-c-text-1, #c9d1d9);
}

.result-count {
  color: var(--vp-c-text-2, #8b949e);
  font-size: 0.95rem;
}

.results-list {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.result-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: var(--vp-c-bg-soft, #161b22);
  border: 1px solid var(--vp-c-divider, #30363d);
  border-left: 3px solid var(--vp-c-divider, #30363d);
  border-radius: 8px;
  padding: 1rem;
  transition: all 0.2s;
}

.result-item.easy {
  border-left-color: #00b8a3;
}

.result-item.medium {
  border-left-color: #ffa116;
}

.result-item.hard {
  border-left-color: #ef4444;
}

.result-item.easy:hover {
  background: var(--vp-c-bg-alt, #1c2128);
  transform: translateX(4px);
  box-shadow: 0 4px 12px rgba(0, 184, 163, 0.15);
}

.result-item.medium:hover {
  background: var(--vp-c-bg-alt, #1c2128);
  transform: translateX(4px);
  box-shadow: 0 4px 12px rgba(255, 161, 22, 0.15);
}

.result-item.hard:hover {
  background: var(--vp-c-bg-alt, #1c2128);
  transform: translateX(4px);
  box-shadow: 0 4px 12px rgba(239, 68, 68, 0.15);
}

.result-content {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  flex: 1;
}

.result-item .problem-name {
  font-size: 1rem;
  font-weight: 500;
  margin-right: 1rem;
}

.problem-topic {
  font-size: 0.85rem;
  color: var(--vp-c-text-3, #6e7681);
}

.no-results {
  text-align: center;
  padding: 3rem 1rem;
  color: var(--vp-c-text-2, #8b949e);
}

.no-results-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.no-results p {
  font-size: 1.1rem;
  margin-bottom: 1.5rem;
}

.clear-search {
  background: #ffa116;
  color: #0d1117;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 600;
  transition: all 0.2s;
}

.clear-search:hover {
  background: #f89b29;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(255, 161, 22, 0.3);
}

@media (max-width: 768px) {
  .lc-container {
    padding: 1rem;
  }

  .lc-header h1 {
    font-size: 1.75rem;
  }

  .lc-controls {
    flex-direction: column;
    align-items: stretch;
  }

  .lc-search {
    max-width: 100%;
  }

  .control-group {
    width: 100%;
    justify-content: space-between;
  }

  .lc-sort {
    flex: 1;
  }

  .lc-filters {
    justify-content: center;
  }

  .stats-grid {
    grid-template-columns: 1fr;
  }
}
</style>
