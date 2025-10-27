<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  resources: {
    type: Array,
    required: true,
    // Expected format: [{ name: 'Resource Name', fileId: 'google-drive-file-id' }]
  },
})

const showModal = ref(false)
const currentPdf = ref(null)
const searchQuery = ref('')
const sortBy = ref('default')
const viewMode = ref('table')
const isFullscreen = ref(false)
const modalContainer = ref(null)

const openPdf = (resource) => {
  currentPdf.value = resource
  showModal.value = true
  document.body.style.overflow = 'hidden'
}

const closeModal = () => {
  if (isFullscreen.value) {
    exitFullscreen()
  }
  showModal.value = false
  currentPdf.value = null
  isFullscreen.value = false
  document.body.style.overflow = 'auto'
}

const getEmbedUrl = (fileId) => {
  return `https://drive.google.com/file/d/${fileId}/preview`
}

const getDownloadUrl = (fileId) => {
  return `https://drive.google.com/uc?export=download&id=${fileId}`
}

const filteredResources = computed(() => {
  let filtered = [...props.resources]
  if (searchQuery.value) {
    filtered = filtered.filter((resource) =>
      resource.name.toLowerCase().includes(searchQuery.value.toLowerCase()),
    )
  }
  if (sortBy.value === 'name') {
    filtered.sort((a, b) => a.name.localeCompare(b.name))
  }
  return filtered
})

const clearSearch = () => {
  searchQuery.value = ''
}

const toggleFullscreen = () => {
  if (!isFullscreen.value) {
    enterFullscreen()
  } else {
    exitFullscreen()
  }
}

const enterFullscreen = () => {
  const element = modalContainer.value

  if (element.requestFullscreen) {
    element.requestFullscreen()
  } else if (element.webkitRequestFullscreen) {
    // Safari
    element.webkitRequestFullscreen()
  } else if (element.mozRequestFullScreen) {
    // Firefox
    element.mozRequestFullScreen()
  } else if (element.msRequestFullscreen) {
    // IE/Edge
    element.msRequestFullscreen()
  }

  isFullscreen.value = true
}

const exitFullscreen = () => {
  if (document.exitFullscreen) {
    document.exitFullscreen()
  } else if (document.webkitExitFullscreen) {
    document.webkitExitFullscreen()
  } else if (document.mozCancelFullScreen) {
    document.mozCancelFullScreen()
  } else if (document.msExitFullscreen) {
    document.msExitFullscreen()
  }

  isFullscreen.value = false
}

const handleFullscreenChange = () => {
  const isCurrentlyFullscreen = !!(
    document.fullscreenElement ||
    document.webkitFullscreenElement ||
    document.mozFullScreenElement ||
    document.msFullscreenElement
  )

  isFullscreen.value = isCurrentlyFullscreen
}

onMounted(() => {
  document.addEventListener('fullscreenchange', handleFullscreenChange)
  document.addEventListener('webkitfullscreenchange', handleFullscreenChange)
  document.addEventListener('mozfullscreenchange', handleFullscreenChange)
  document.addEventListener('MSFullscreenChange', handleFullscreenChange)
})

onBeforeUnmount(() => {
  document.removeEventListener('fullscreenchange', handleFullscreenChange)
  document.removeEventListener('webkitfullscreenchange', handleFullscreenChange)
  document.removeEventListener('mozfullscreenchange', handleFullscreenChange)
  document.removeEventListener('MSFullscreenChange', handleFullscreenChange)
})
</script>

<template>
  <div class="resources-wrapper">
    <!-- Toolbar -->
    <div class="toolbar">
      <div class="search-box">
        <svg
          class="search-icon"
          width="16"
          height="16"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
        >
          <circle cx="11" cy="11" r="8"></circle>
          <path d="m21 21-4.35-4.35"></path>
        </svg>
        <input
          v-model="searchQuery"
          type="text"
          placeholder="Search resources..."
          class="search-input"
        />
        <button v-if="searchQuery" @click="clearSearch" class="clear-btn">
          <svg
            width="14"
            height="14"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
          </svg>
        </button>
      </div>

      <div class="toolbar-actions">
        <select v-model="sortBy" class="sort-select">
          <option value="default">Default Order</option>
          <option value="name">Sort by Name</option>
        </select>

        <div class="view-toggle">
          <button
            :class="['view-btn', { active: viewMode === 'table' }]"
            @click="viewMode = 'table'"
            title="Table view"
          >
            <svg
              width="16"
              height="16"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
            >
              <line x1="8" y1="6" x2="21" y2="6"></line>
              <line x1="8" y1="12" x2="21" y2="12"></line>
              <line x1="8" y1="18" x2="21" y2="18"></line>
              <line x1="3" y1="6" x2="3.01" y2="6"></line>
              <line x1="3" y1="12" x2="3.01" y2="12"></line>
              <line x1="3" y1="18" x2="3.01" y2="18"></line>
            </svg>
          </button>
          <button
            :class="['view-btn', { active: viewMode === 'grid' }]"
            @click="viewMode = 'grid'"
            title="Grid view"
          >
            <svg
              width="16"
              height="16"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
            >
              <rect x="3" y="3" width="7" height="7"></rect>
              <rect x="14" y="3" width="7" height="7"></rect>
              <rect x="14" y="14" width="7" height="7"></rect>
              <rect x="3" y="14" width="7" height="7"></rect>
            </svg>
          </button>
        </div>
      </div>
    </div>

    <!-- Empty state -->
    <div v-if="filteredResources.length === 0" class="empty-state">
      <svg
        width="48"
        height="48"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
      >
        <circle cx="11" cy="11" r="8"></circle>
        <path d="m21 21-4.35-4.35"></path>
      </svg>
      <p>No resources found</p>
    </div>

    <!-- Table View -->
    <table
      v-if="viewMode === 'table' && filteredResources.length > 0"
      class="resources-table"
    >
      <thead>
        <tr>
          <th>#</th>
          <th>Resource</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(resource, index) in filteredResources"
          :key="index"
          class="table-row"
        >
          <td>{{ index + 1 }}</td>
          <td>
            <div class="resource-name">
              <svg
                class="file-icon"
                width="16"
                height="16"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <path
                  d="M13 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V9z"
                ></path>
                <polyline points="13 2 13 9 20 9"></polyline>
              </svg>
              {{ resource.name }}
            </div>
          </td>
          <td>
            <div class="action-buttons">
              <button
                class="btn btn-view"
                @click="openPdf(resource)"
                title="View PDF"
              >
                <svg
                  width="16"
                  height="16"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                  <circle cx="12" cy="12" r="3"></circle>
                </svg>
                View
              </button>
              <a
                :href="getDownloadUrl(resource.fileId)"
                class="btn btn-download"
                download
                title="Download PDF"
              >
                <svg
                  width="16"
                  height="16"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
                  <polyline points="7 10 12 15 17 10"></polyline>
                  <line x1="12" y1="15" x2="12" y2="3"></line>
                </svg>
                Download
              </a>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Grid View -->
    <div
      v-if="viewMode === 'grid' && filteredResources.length > 0"
      class="resources-grid"
    >
      <div
        v-for="(resource, index) in filteredResources"
        :key="index"
        class="grid-card"
      >
        <div class="card-header">
          <div class="card-number">{{ index + 1 }}</div>
          <svg
            class="card-icon"
            width="32"
            height="32"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              d="M13 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V9z"
            ></path>
            <polyline points="13 2 13 9 20 9"></polyline>
          </svg>
        </div>
        <div class="card-body">
          <h4 class="card-title">{{ resource.name }}</h4>
        </div>
        <div class="card-actions">
          <button
            class="btn btn-view"
            @click="openPdf(resource)"
            title="View PDF"
          >
            <svg
              width="16"
              height="16"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
            >
              <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
              <circle cx="12" cy="12" r="3"></circle>
            </svg>
            View
          </button>
          <a
            :href="getDownloadUrl(resource.fileId)"
            class="btn btn-download"
            download
            title="Download PDF"
          >
            <svg
              width="16"
              height="16"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
            >
              <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
              <polyline points="7 10 12 15 17 10"></polyline>
              <line x1="12" y1="15" x2="12" y2="3"></line>
            </svg>
            Download
          </a>
        </div>
      </div>
    </div>
  </div>

  <!-- Modal -->
  <Transition name="modal">
    <div v-if="showModal" class="modal-overlay" @click="closeModal">
      <div
        ref="modalContainer"
        class="modal-container"
        @click.stop
        :class="{ fullscreen: isFullscreen }"
      >
        <div class="modal-header">
          <h3>{{ currentPdf?.name }}</h3>
          <div class="modal-actions">
            <!-- Fullscreen button -->
            <button
              class="btn-icon"
              @click="toggleFullscreen"
              :title="isFullscreen ? 'Exit Fullscreen' : 'Fullscreen'"
            >
              <svg
                v-if="!isFullscreen"
                width="20"
                height="20"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <path
                  d="M8 3H5a2 2 0 0 0-2 2v3m18 0V5a2 2 0 0 0-2-2h-3m0 18h3a2 2 0 0 0 2-2v-3M3 16v3a2 2 0 0 0 2 2h3"
                ></path>
              </svg>
              <svg
                v-else
                width="20"
                height="20"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <path
                  d="M8 3v3a2 2 0 0 1-2 2H3m18 0h-3a2 2 0 0 1-2-2V3m0 18v-3a2 2 0 0 1 2-2h3M3 16h3a2 2 0 0 1 2 2v3"
                ></path>
              </svg>
            </button>

            <!-- Download button -->
            <a
              :href="getDownloadUrl(currentPdf?.fileId)"
              class="btn-icon"
              download
              title="Download"
            >
              <svg
                width="20"
                height="20"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
                <polyline points="7 10 12 15 17 10"></polyline>
                <line x1="12" y1="15" x2="12" y2="3"></line>
              </svg>
            </a>

            <!-- Close button -->
            <button
              class="btn-icon btn-close"
              @click="closeModal"
              title="Close"
            >
              <svg
                width="20"
                height="20"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <line x1="18" y1="6" x2="6" y2="18"></line>
                <line x1="6" y1="6" x2="18" y2="18"></line>
              </svg>
            </button>
          </div>
        </div>
        <div class="modal-body">
          <iframe
            v-if="currentPdf"
            :src="getEmbedUrl(currentPdf.fileId)"
            frameborder="0"
            allowfullscreen
          ></iframe>
        </div>
      </div>
    </div>
  </Transition>
</template>

<style scoped>
.resources-wrapper {
  margin: 16px 0;
}

/* Toolbar */
.toolbar {
  display: flex;
  gap: 12px;
  margin-bottom: 16px;
  flex-wrap: wrap;
  align-items: center;
}

.search-box {
  position: relative;
  flex: 1;
  min-width: 250px;
}

.search-icon {
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: var(--vp-c-text-3);
  pointer-events: none;
}

.search-input {
  width: 100%;
  padding: 8px 36px 8px 36px;
  border: 1px solid var(--vp-c-divider);
  border-radius: 8px;
  background: var(--vp-c-bg);
  color: var(--vp-c-text-1);
  font-size: 14px;
  transition: all 0.2s ease;
}

.search-input:focus {
  outline: none;
  border-color: var(--vp-c-brand);
  box-shadow: 0 0 0 3px var(--vp-c-brand-soft);
}

.clear-btn {
  position: absolute;
  right: 8px;
  top: 50%;
  transform: translateY(-50%);
  padding: 4px;
  border: none;
  background: transparent;
  color: var(--vp-c-text-3);
  cursor: pointer;
  border-radius: 4px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.clear-btn:hover {
  background: var(--vp-c-bg-soft);
  color: var(--vp-c-text-1);
}

.toolbar-actions {
  display: flex;
  gap: 8px;
  align-items: center;
}

.sort-select {
  padding: 8px 12px;
  border: 1px solid var(--vp-c-divider);
  border-radius: 8px;
  background: var(--vp-c-bg);
  color: var(--vp-c-text-1);
  font-size: 14px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.sort-select:hover {
  border-color: var(--vp-c-brand);
}

.sort-select:focus {
  outline: none;
  border-color: var(--vp-c-brand);
  box-shadow: 0 0 0 3px var(--vp-c-brand-soft);
}

.view-toggle {
  display: flex;
  border: 1px solid var(--vp-c-divider);
  border-radius: 8px;
  overflow: hidden;
}

.view-btn {
  padding: 8px 12px;
  border: none;
  background: var(--vp-c-bg);
  color: var(--vp-c-text-2);
  cursor: pointer;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.view-btn:not(:last-child) {
  border-right: 1px solid var(--vp-c-divider);
}

.view-btn:hover {
  background: var(--vp-c-bg-soft);
  color: var(--vp-c-text-1);
}

.view-btn.active {
  background: var(--vp-c-brand-soft);
  color: var(--vp-c-brand);
}

/* Empty State */
.empty-state {
  text-align: center;
  padding: 48px 24px;
  color: var(--vp-c-text-3);
}

.empty-state svg {
  margin-bottom: 16px;
  opacity: 0.5;
}

.empty-state p {
  margin: 0;
  font-size: 16px;
}

/* Table View */
.resources-table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid var(--vp-c-divider);
  overflow: hidden;
  border-radius: 8px;
}

.resources-table thead {
  background: var(--vp-c-bg-soft);
  border-bottom: 1px solid var(--vp-c-divider);
}

.resources-table th {
  padding: 8px 16px;
  text-align: left;
  font-weight: 600;
  color: var(--vp-c-text-1);
  border-bottom: 1px solid var(--vp-c-divider);
}

.resources-table th:first-child {
  text-align: center;
  width: 60px;
}

.resources-table th:last-child {
  text-align: center;
  width: auto;
}

.resources-table td {
  padding: 6px 16px;
  border-top: 1px solid var(--vp-c-divider);
  color: var(--vp-c-text-2);
}

.resources-table td:first-child {
  text-align: center;
  font-weight: 600;
  color: var(--vp-c-text-1);
}

.table-row {
  transition: background 0.2s ease;
  animation: slideIn 0.3s ease forwards;
  opacity: 0;
}

.table-row:nth-child(1) {
  animation-delay: 0.05s;
}
.table-row:nth-child(2) {
  animation-delay: 0.1s;
}
.table-row:nth-child(3) {
  animation-delay: 0.15s;
}
.table-row:nth-child(4) {
  animation-delay: 0.2s;
}
.table-row:nth-child(5) {
  animation-delay: 0.25s;
}
.table-row:nth-child(n + 6) {
  animation-delay: 0.3s;
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

.table-row:hover {
  background: var(--vp-c-bg-soft);
}

.resource-name {
  display: flex;
  align-items: center;
  gap: 8px;
}

.file-icon {
  color: var(--vp-c-brand);
  flex-shrink: 0;
}

/* Grid View */
.resources-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 16px;
}

.grid-card {
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  padding: 16px;
  background: var(--vp-c-bg);
  transition: all 0.3s ease;
  animation: fadeIn 0.4s ease forwards;
  opacity: 0;
}

.grid-card:nth-child(1) {
  animation-delay: 0.05s;
}
.grid-card:nth-child(2) {
  animation-delay: 0.1s;
}
.grid-card:nth-child(3) {
  animation-delay: 0.15s;
}
.grid-card:nth-child(4) {
  animation-delay: 0.2s;
}
.grid-card:nth-child(5) {
  animation-delay: 0.25s;
}
.grid-card:nth-child(6) {
  animation-delay: 0.3s;
}
.grid-card:nth-child(n + 7) {
  animation-delay: 0.35s;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.grid-card:hover {
  border-color: var(--vp-c-brand);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transform: translateY(-2px);
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.card-number {
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--vp-c-brand-soft);
  color: var(--vp-c-brand);
  border-radius: 8px;
  font-weight: 600;
  font-size: 14px;
}

.card-icon {
  color: var(--vp-c-text-3);
}

.card-body {
  margin-bottom: 16px;
}

.card-title {
  margin: 0;
  font-size: 15px;
  font-weight: 500;
  color: var(--vp-c-text-1);
  line-height: 1.5;
}

.card-actions {
  display: flex;
  gap: 8px;
}

.card-actions .btn {
  flex: 1;
  justify-content: center;
}

/* Buttons */
.action-buttons {
  display: flex;
  gap: 6px;
  justify-content: center;
}

.btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 6px 12px;
  border: 1px solid var(--vp-c-divider);
  border-radius: 6px;
  font-size: 13px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
  text-decoration: none;
}

.btn-view {
  background: var(--vp-c-brand-soft);
  color: var(--vp-c-brand);
}

.btn-view:hover {
  background: var(--vp-c-brand);
  color: var(--vp-c-bg);
  border-color: var(--vp-c-brand);
}

.btn-download {
  background: var(--vp-c-bg);
  color: var(--vp-c-text-2);
}

.btn-download:hover {
  background: var(--vp-c-bg-soft);
  border-color: var(--vp-c-brand);
  color: var(--vp-c-brand);
}

/* Modal Styles */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.75);
  backdrop-filter: blur(4px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  padding: 20px;
}

.modal-container {
  background: var(--vp-c-bg);
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  width: 90vw;
  max-width: 1200px;
  height: 90vh;
  display: flex;
  flex-direction: column;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}

/* Fullscreen styles */
.modal-container.fullscreen,
.modal-container:fullscreen,
.modal-container:-webkit-full-screen,
.modal-container:-moz-full-screen,
.modal-container:-ms-fullscreen {
  width: 100vw !important;
  height: 100vh !important;
  max-width: 100vw !important;
  max-height: 100vh !important;
  margin: 0 !important;
  border-radius: 0 !important;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 24px;
  background: var(--vp-c-bg-soft);
  border-bottom: 1px solid var(--vp-c-divider);
  flex-shrink: 0;
}

.modal-header h3 {
  margin: 0;
  color: var(--vp-c-text-1);
  font-size: 18px;
  font-weight: 600;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  max-width: calc(100% - 150px);
}

.modal-actions {
  display: flex;
  gap: 8px;
  flex-shrink: 0;
}
.btn-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  border: none;
  background: transparent;
  color: var(--vp-c-text-2);
  cursor: pointer;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.btn-icon:hover {
  background: var(--vp-c-bg-soft);
  color: var(--vp-c-brand);
}

.btn-close:hover {
  background: rgba(239, 68, 68, 0.1);
  color: #ef4444;
}

.modal-body {
  flex: 1;
  overflow: hidden;
}

.modal-body iframe {
  width: 100%;
  height: 100%;
}

/* Modal Transitions */
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;
}

.modal-enter-active .modal-container,
.modal-leave-active .modal-container {
  transition: transform 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  transform: scale(0.95);
}

/* Responsive */
@media (max-width: 768px) {
  .toolbar {
    flex-direction: column;
    align-items: stretch;
  }

  .search-box {
    min-width: 100%;
  }

  .toolbar-actions {
    justify-content: space-between;
  }

  .resources-grid {
    grid-template-columns: 1fr;
  }
}
</style>
