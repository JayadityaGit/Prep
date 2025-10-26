<template>
  <div class="contrib-card">
    <!-- SVG golden outline -->
    <svg
      class="outline-svg"
      viewBox="0 0 350 500"
      preserveAspectRatio="none"
      aria-hidden="true"
      focusable="false"
    >
      <defs>
        <linearGradient id="goldGrad" x1="0" x2="1" y1="0" y2="1">
          <stop offset="0%" stop-color="#ffd8a8" />
          <stop offset="40%" stop-color="#f2a85a" />
          <stop offset="100%" stop-color="#dd8448" />
        </linearGradient>
      </defs>
      <rect
        x="2"
        y="2"
        width="370"
        height="500"
        rx="24"
        ry="24"
        fill="none"
        stroke="url(#goldGrad)"
        stroke-width="3"
        stroke-linejoin="round"
        class="outline-rect"
      />
    </svg>

    <!-- subtle outer glow -->
    <div class="outer-glow" aria-hidden="true"></div>

    <!-- content -->
    <div class="content">
      <div class="top">
        <div class="badge">{{ badge }}</div>

        <div class="avatar-container">
          <div class="avatar-glow"></div>
          <div class="avatar-wrap" v-if="image">
            <img :src="image" alt="" class="avatar" />
          </div>
          <div class="avatar-wrap placeholder" v-else>
            <div class="avatar-initials">{{ initials }}</div>
          </div>
        </div>

        <h3 class="name">{{ title }}</h3>
        <div class="role" v-if="role">{{ role }}</div>
      </div>


      <div class="bottom">
        <p class="desc">{{ description }}</p>

        <div class="links" v-if="links && links.length">
          <a
            v-for="(link, idx) in links"
            :key="idx"
            :href="link.url"
            class="link-btn"
            target="_blank"
            rel="noopener noreferrer"
          >
            <img
              :src="getIcon(link.label)"
              :alt="link.label + ' logo'"
              class="link-icon"
            />
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  badge: { type: String, default: 'Dramatic' },
  title: { type: String, default: 'Electric Border' },
  description: {
    type: String,
    default: "In case you'd like to emphasize something very dramatically.",
  },
  image: { type: String, default: '' },
  role: { type: String, default: '' },
  links: { type: Array, default: () => [] },
})

const getIcon = (label) => {
  const lower = label.toLowerCase()
  if (lower.includes('github'))
    return 'https://cdn.jsdelivr.net/gh/simple-icons/simple-icons/icons/github.svg'
  if (lower.includes('linkedin'))
    return 'https://cdn.jsdelivr.net/gh/simple-icons/simple-icons/icons/linkedin.svg'
  if (lower.includes('leetcode'))
    return 'https://cdn.jsdelivr.net/gh/simple-icons/simple-icons/icons/leetcode.svg'
  if (lower.includes('codeforces'))
    return 'https://cdn.jsdelivr.net/gh/simple-icons/simple-icons/icons/codeforces.svg'
  if (lower.includes('reddit'))
    return 'https://cdn.jsdelivr.net/gh/simple-icons/simple-icons/icons/reddit.svg'
  if (lower.includes('monkeytype'))
    return 'https://cdn.jsdelivr.net/gh/simple-icons/simple-icons/icons/monkeytype.svg'
  if (lower.includes('matiks'))
    return 'https://res.cloudinary.com/dzjxhzxym/image/upload/v1761233815/Screenshot_from_2025-10-23_21-05-44_rrjftz.png'
  return 'https://cdn.jsdelivr.net/gh/simple-icons/simple-icons/icons/link.svg'
}

// compute initials from title if no image provided
import { computed } from 'vue'
const initials = computed(() => {
  const name = props.title || ''
  return name
    .split(' ')
    .map((s) => s.charAt(0).toUpperCase())
    .slice(0, 2)
    .join('')
})
</script>

<style scoped>
/* ---------- THEME VARIABLES ---------- */
:root {
  /* sizes / layout */
  --card-max-w: 350px;
  --radius: 24px;

  /* gold gradient */
  --gold-start: #ffd8a8;
  --gold-mid: #f2a85a;
  --gold-end: #dd8448;

  /* Dark-mode colors (default) */
  --card-bg-top: rgba(255, 255, 255, 0.02);
  --card-bg-bottom: rgba(26, 26, 28, 0.95);
  --text-main: #ffffff;
  --text-muted: rgba(255, 255, 255, 0.75);
  --desc-muted: rgba(255,255,255,0.55);
  --avatar-bg: rgba(255,255,255,0.02);
  --card-box-shadow: 0 12px 40px rgba(0, 0, 0, 0.6);
  --card-inset-shadow: inset 0 6px 40px rgba(0, 0, 0, 0.35);
  --outer-glow-bg: linear-gradient(22deg, rgba(217,217,217,0.41), #9f90905e 94%, rgba(221,132,72,0.06));
  --outer-glow-opacity: 0.42;
  --outline-shadow-color: rgba(221, 132, 72, 0.12);
  --avatar-boxshadow: 0 6px 20px rgba(0, 0, 0, 0.6);
  --link-bg: rgba(255,255,255,0.04);
  --link-border: rgba(255,255,255,0.03);
  --link-hover-bg: rgba(255,255,255,0.06);
  --link-icon-filter: invert(0.9);
  --badge-bg: rgba(255, 255, 255, 0.06);
  --badge-text: rgba(255, 255, 255, 0.9);
  --badge-border: rgba(255, 255, 255, 0.06);
  --divider-bg: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.04), transparent);
  --avatar-glow-color: rgba(242, 168, 90, 0.4);
}

/* Light theme overrides */
html[data-theme="light"] :root,
body[data-theme="light"] :root,
:root.light-theme {
  --card-bg-top: rgba(255,255,255,0.95);
  --card-bg-bottom: rgba(250,250,251,1);
  --text-main: #111827;
  --text-muted: rgba(17,24,39,0.75);
  --desc-muted: rgba(17,24,39,0.65);
  --avatar-bg: rgba(0,0,0,0.02);
  --card-box-shadow: 0 20px 40px rgba(0, 0, 0, 0.08);
  --card-inset-shadow: inset 0 4px 24px rgba(0,0,0,0.02);
  --outer-glow-bg: linear-gradient(22deg, rgba(255,220,180,0.6), rgba(242,168,90,0.15) 80%);
  --outer-glow-opacity: 0.7;
  --outline-shadow-color: rgba(221, 132, 72, 0.2);
  --avatar-boxshadow: 0 12px 28px rgba(0, 0, 0, 0.08);
  --link-bg: rgba(0,0,0,0.03);
  --link-border: rgba(0,0,0,0.08);
  --link-hover-bg: rgba(0,0,0,0.06);
  --link-icon-filter: invert(0.1) brightness(0.3);
  --badge-bg: rgba(0,0,0,0.06);
  --badge-text: rgba(17,24,39,0.9);
  --badge-border: rgba(0,0,0,0.08);
  --divider-bg: linear-gradient(to right, transparent, rgba(0,0,0,0.08), transparent);
  --avatar-glow-color: rgba(242, 168, 90, 0.3);
}

/* Card wrapper (responsive) */
.contrib-card {
  width: 100%;
  max-width: var(--card-max-w);
  aspect-ratio: 7 / 10;
  position: relative;
  margin: 0 auto;
  box-sizing: border-box;
  font-family: system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
}

/* outer blur/glow */
.outer-glow {
  position: absolute;
  inset: 0;
  border-radius: var(--radius);
  z-index: 0;
  filter: blur(28px);
  opacity:.1;
  background: white;
  transform: translate(33px, 58px);
  pointer-events: none;
  height: 260px;
  width: 263px;
  border-radius: 50%;
}

.content {
  position: absolute;
  inset: 6px;
  align-items: center;
  border-radius: calc(var(--radius) - 4px);
  z-index: 2;
  display: flex;
  flex-direction: column;
  background:
    linear-gradient(-30deg, var(--card-bg-top), rgba(0,0,0,0)),
    linear-gradient(to bottom, var(--card-bg-top), var(--card-bg-bottom));
  box-shadow: var(--card-box-shadow), var(--card-inset-shadow);
  color: var(--text-main);
}

/* outline svg */
.outline-svg {
  position: absolute;
  inset: 0;
  width: 94%;
  height: 100%;
  z-index: 3;
  pointer-events: none;
  overflow: visible;
}

.outline-rect {
  filter: drop-shadow(0 6px 18px var(--outline-shadow-color));
}

/* top area */
.top {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 15px 14px 12px 10px;
    gap: 12px;
    width: 100%;
}

.badge {
  display: inline-block;
  background: var(--badge-bg);
  color: var(--badge-text);
  padding: 8px 14px;
  border-radius: 999px;
  font-weight: 700;
  font-size: 12px;
  letter-spacing: 0.6px;
  text-transform: uppercase;
  border: 1px solid var(--badge-border);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.02);
}

/* avatar container with glow */
.avatar-container {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 6px;
}

.avatar-glow {
  position: absolute;
  width: 180px;
  height: 180px;
  border-radius: 50%;
  background: radial-gradient(circle, var(--avatar-glow-color) 0%, transparent 70%);
  filter: blur(20px);
  z-index: 0;
  pointer-events: none;
  opacity: 0.6;
}

/* avatar */
.avatar-wrap {
  width: 140px;
  height: 140px;
  border-radius: 999px;
  overflow: hidden;
  display: grid;
  place-items: center;
  box-shadow: var(--avatar-boxshadow), inset 0 0 0 6px rgba(255,255,255,0.02);
  background: var(--avatar-bg);
  position: relative;
  z-index: 1;
}

.avatar {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

/* placeholder initials */
.avatar-wrap.placeholder {
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.02), rgba(255, 255, 255, 0.01));
}

.avatar-initials {
  color: var(--text-main);
  font-size: 34px;
  font-weight: 600;
}


.name {
      margin: 6px 0 0 0;
    font-size: 32px;
    line-height: 1;
    color: var(--text-main);
    font-weight: 600;
    display: flex;
    align-items: center;
    text-align: center;
}

.role {
  margin-top: 6px;
  font-size: 15px;
  color: var(--text-muted);
  text-align: center;
}

/* divider */
.divider {
  margin: 12px 24px;
  border: none;
  height: 1px;
  background: var(--divider-bg);
  opacity: 0.5;
}

.bottom {
      position: relative;
    z-index: 5;
    padding: 13px 9px 13px 9px;
    display: flex
;
    flex-direction: column;
    justify-content: flex-start;
    flex: 1 1 auto;
    align-items: center;
}

.desc {
  margin: 0;
  color: var(--desc-muted);
  font-size: 14px;
  line-height: 1.6;
  text-align: center;
}




/* links row */
.links {
  margin-top: 14px;
  display: flex;
  gap: 8px;
  flex-wrap: nowrap;
  justify-content: center;
}

.link-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 6px 10px;
  border-radius: 8px;
  background: #ff9c2a5c;
  color: var(--text-main);
  font-size: 13px;
  text-decoration: none;
  border: 1px solid var(--link-border);
  transition: transform 0.12s ease, background 0.12s ease;
}
html[data-theme="light"] .link-btn {
  border: 1px solid rgba(0,0,0,0.08);
  background: rgba(9,7,7,0.16);
}

.badge {
  display: inline-block;
  background: var(--badge-bg);
  color: var(--badge-text);
  padding: 8px 16px;
  border-radius: 999px;
  font-weight: 700;
  font-size: 12px;
  letter-spacing: 0.8px;
  text-transform: uppercase;
  border: 2px solid transparent;
  background-image: 
    linear-gradient(var(--badge-bg), var(--badge-bg)),
    linear-gradient(135deg, var(--gold-start), var(--gold-mid), var(--gold-end));
  background-origin: border-box;
  background-clip: padding-box, border-box;
  box-shadow: 
    inset 0 1px 2px rgba(255, 255, 255, 0.1),
    0 2px 8px rgba(221, 132, 72, 0.15),
    0 0 0 1px rgba(255, 255, 255, 0.03);
  position: relative;
  overflow: hidden;
}

.badge::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: 999px;
  padding: 2px;
  background: linear-gradient(135deg, var(--gold-start), var(--gold-mid), var(--gold-end));
  -webkit-mask: 
    linear-gradient(#fff 0 0) content-box, 
    linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
  opacity: 0.6;
  pointer-events: none;
}
.link-btn:hover {
  transform: translateY(-2px);
  background: var(--link-hover-bg);
}

.link-icon {
  width: 16px;
  height: 16px;
  filter: var(--link-icon-filter);
}

/* ---------- RESPONSIVE BREAKPOINTS ---------- */

/* Tablet and mobile */
@media (max-width: 768px) {
  .contrib-card {
    max-width: 300px;
    margin: 0 auto;
  }
  .outer-glow {
 
  opacity:.1;
  background: white;
  transform: translate(14px, 29px);

}
  .avatar-glow {
    width: 150px;
    height: 150px;
  }

  .avatar-wrap {
    width: 110px;
    height: 110px;
  }
  
  .name {
    font-size: 24px;
  }
  
  .role {
    font-size: 13px;
  }
  
  .top {
    padding: 28px 18px 10px 18px;
  }
  
  .bottom {
    padding: 14px 18px 18px 18px;
    
  }
  
  .desc {
    font-size: 13px;
  }
  
  .badge {
    font-size: 11px;
    padding: 6px 12px;
  }
  
  .links {
    gap: 6px;
  }
  
  .link-btn {
    padding: 5px 8px;
  }
  
  .link-icon {
    width: 20px;
    height: 20px;
  }
}
.desc{
  font-weight:500;
}
/* Small mobile */
@media (max-width: 420px) {
  .contrib-card {
    max-width: 280px;
  }
  
  .avatar-glow {
    width: 130px;
    height: 130px;
  }
  
  .avatar-wrap {
    width: 100px;
    height: 100px;
  }
  
  .name {
    font-size: 22px;
  }
  
  .role {
    font-size: 12px;
  }
  
  .desc {
    font-size: 12px;
  }
  
  .top {
    padding: 24px 16px 8px 16px;
    gap: 10px;
  }
  
  .bottom {
    padding: 12px 16px 16px 16px;
   
  }
}
</style>