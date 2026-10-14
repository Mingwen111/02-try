<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue'
import heroImage from './assets/hero-figma-1.png'
import sectionImage from './assets/spot-ornate-gate.jpg'
import spotForbiddenCity from './assets/spot-forbidden-city.jpg'
import spotTempleWater from './assets/spot-temple-water.jpg'
import spotRedGateway from './assets/spot-red-gateway.jpg'
import spotRedGate from './assets/spot-red-gate.jpg'
import spotOrnateGate from './assets/spot-ornate-gate.jpg'
import arrowIcon from './assets/arrow-right.svg'

const pages = [
  { path: '/', label: '首页' },
  { path: '/scenic-spots', label: '景点大全' },
  { path: '/travel-guides', label: '旅游攻略' },
  { path: '/news', label: '新闻资讯' },
  { path: '/booking', label: '预约预定' },
  { path: '/contact', label: '联系我们' },
  { path: '/test', label: '测试' },
]

const pageLabels = {
  '/': '首页',
  '/scenic-spots': '景点大全',
  '/travel-guides': '旅游攻略',
  '/news': '新闻资讯',
  '/booking': '预约预定',
  '/contact': '联系我们',
  '/test': '测试',
}

const currentPath = ref(window.location.hash.slice(1) || '/')
const menuOpen = ref(false)
const currentPage = computed(() => pageLabels[currentPath.value] ? currentPath.value : '/')
const currentLabel = computed(() => pageLabels[currentPage.value])
const hoveredSpot = ref(null)
const spotPointer = ref({ x: 0, y: 0 })

const colorScales = [
  {
    name: '主色 Primary',
    colors: ['#fbeff0', '#f5d9da', '#edb2b5', '#c94e54', '#9c2c2f', '#8a272a', '#7b2225', '#671c1f', '#511619', '#3a0f11'],
  },
  {
    name: '辅助色 Secondary',
    colors: ['#fcf8ee', '#f6ebd5', '#eedbb8', '#e0be80', '#d0a252', '#b88b43', '#9e7337', '#815d2d', '#664923', '#493319'],
  },
  {
    name: '灰色 Gray',
    colors: ['#faf9f8', '#f1efee', '#dedbda', '#b5b1b0', '#817e7e', '#6d6969', '#595555', '#464343', '#333131', '#211f1f'],
  },
]

const semanticColors = [
  { name: '成功 Success', value: '#5f8067' },
  { name: '警告 Warning', value: '#d0a252' },
  { name: '错误 Error', value: '#9c2c2f' },
  { name: '信息 Info', value: '#617a85' },
]

const colorUsageRules = [
  { value: '80%', title: '灰色系', text: '用于页面背景、正文、边框、分隔线和大部分界面结构。' },
  { value: '15%', title: '主色', text: '用于品牌标识、当前导航、关键行动和少量视觉焦点。' },
  { value: '5%', title: '其余颜色', text: '用于辅助强调、状态提示和信息分组，避免抢夺主色层级。' },
]

const coreSpots = [
  { name: '风度学校', image: spotRedGateway },
  { name: '张发奎故居', image: spotRedGate },
  { name: '风度革命历史陈列馆', image: spotOrnateGate },
  { name: '中共始兴县委旧址、东江纵队和风度大队会师旧址', image: spotForbiddenCity },
  { name: '宣誓台', image: heroImage },
  { name: '练兵场', image: spotTempleWater },
  { name: '幸相粉竹体验馆', image: spotOrnateGate },
]

const galleryPhotos = [spotRedGateway, spotRedGate, spotOrnateGate, spotForbiddenCity, heroImage]

function navigate(path) {
  window.location.hash = path
  menuOpen.value = false
}

function syncRoute() {
  currentPath.value = window.location.hash.slice(1) || '/'
}

function updateSpotPointer(event) {
  const bounds = event.currentTarget.getBoundingClientRect()
  spotPointer.value = {
    x: event.clientX - bounds.left,
    y: event.clientY - bounds.top,
  }
}

function showSpotPointer(name, event) {
  hoveredSpot.value = name
  updateSpotPointer(event)
}

function hideSpotPointer(name) {
  if (hoveredSpot.value === name) hoveredSpot.value = null
}

onMounted(() => window.addEventListener('hashchange', syncRoute))
onUnmounted(() => window.removeEventListener('hashchange', syncRoute))
</script>

<template>
  <div class="site-shell">
    <header class="site-header">
      <a class="brand" href="#/" aria-label="返回首页" @click="navigate('/')">
        <span class="brand-mark">红</span>
        <span><strong>红色风度</strong><small>遵义旅游官网</small></span>
      </a>

      <button class="menu-toggle" type="button" :aria-expanded="menuOpen" aria-label="打开导航菜单" @click="menuOpen = !menuOpen">
        <span></span><span></span>
      </button>

      <nav class="top-nav" :class="{ 'is-open': menuOpen }" aria-label="顶部导航">
        <a v-for="page in pages" :key="page.path" :href="`#${page.path}`" :class="{ active: currentPage === page.path }" @click="navigate(page.path)">
          {{ page.label }}
        </a>
      </nav>
    </header>

    <main class="page-frame" :class="{ 'is-home-page': currentPage === '/', 'is-test-page': currentPage === '/test' }">
      <template v-if="currentPage === '/'">
        <section class="site-section section--auto home-hero" aria-labelledby="home-hero-title">
          <div class="hero-copy">
            <p class="hero-badge type-24">国家级AAA景区（创建中）</p>
            <h1 id="home-hero-title" class="type-78 hero-title">风度村红色旅游区</h1>
            <p class="type-36 hero-subtitle">始兴革命摇篮 · 红色研学基地</p>
            <p class="type-16 hero-description">风度村素有“粤北革命摇篮”之称，曾是抗战时期始兴县委机关和抗日指挥中心所在地，村里自然环境优美，红色文化资源丰富。探寻红色革命旧址，见证守家卫国历史，开启一场爱国精神的熏陶之旅。</p>
          </div>
          <figure class="hero-media">
            <img :src="heroImage" alt="红色风景区景观" />
          </figure>
        </section>
        <section class="site-section section--auto open-info-section" aria-labelledby="open-info-title">
          <h2 id="open-info-title" class="type-52 open-info-title">景区介绍</h2>

          <div class="open-info-label open-info-label--hours">
            <h3 class="type-36">开放时间</h3>
          </div>

          <div class="open-info-hours">
            <div class="open-info-hours-row open-info-hours-row--outdoor">
              <span class="type-24">户外景点</span>
              <span class="type-16 open-info-muted">全天开放</span>
            </div>
            <div class="open-info-hours-row">
              <span class="type-20">风度学校 &amp; 风度革命历史陈列馆</span>
              <span class="type-16 open-info-muted">周二至周日</span>
              <span class="type-16 open-info-muted">9:00—17:00（16:30停止入场）</span>
              <span class="type-16 open-info-muted">周一</span>
              <span class="type-16 open-info-muted">闭馆（法定节假日除外）</span>
            </div>
          </div>

          <figure class="open-info-image open-info-image--top">
            <img :src="sectionImage" alt="风度村红色旅游区景观" />
          </figure>

          <div class="open-info-rule" aria-hidden="true"></div>

          <div class="open-info-label open-info-label--history">
            <h3 class="type-36">历史介绍</h3>
          </div>

          <div class="open-info-history">
            <p class="type-16">风度村红色旅游区，位于韶关市始兴县隘子镇，素有“粤北革命摇篮”之称，是抗战时期中共始兴县委机关和抗日指挥中心所在地。核心遗址风度学校建于1936年，由抗日名将张发奎出资创办，邓文礼、吴新民等30余名中共地下党员曾在此任教，使学校实际成为始兴党组织领导抗日救亡运动的中心。1945年，东江纵队北上部队与风度大队在此会师，留下“东江纵队会师地旧址”等重要遗迹。作为广东省首批“红色村党建示范点”，景区已完成风度学校、中共始兴县委旧址的“修旧如旧”修复，并建有风度革命历史陈列馆、红色主题广场等设施，现为韶关市爱国主义教育基地和党史教育基地，是一处集红色研学、党建教育与文旅休闲于一体的特色旅游区。</p>
            <a class="open-info-cta" href="#/scenic-spots" @click="navigate('/scenic-spots')">查看更多 <span aria-hidden="true">↗</span></a>
          </div>

          <figure class="open-info-image open-info-image--bottom">
            <img :src="sectionImage" alt="风度村红色旅游区景观" />
          </figure>
        </section>
        <section class="site-section core-spots-section" aria-labelledby="core-spots-title">
          <h2 id="core-spots-title" class="type-52 core-spots-title">核心景点</h2>
          <div class="core-spot-list">
            <article v-for="spot in coreSpots" :key="spot.name" class="core-spot-item">
              <h3 class="type-36 core-spot-label">{{ spot.name }}</h3>
              <div
                class="core-spot-media"
                @mouseenter="showSpotPointer(spot.name, $event)"
                @mousemove="updateSpotPointer"
                @mouseleave="hideSpotPointer(spot.name)"
              >
                <img :src="spot.image" :alt="spot.name" />
                <span
                  v-if="hoveredSpot === spot.name"
                  class="core-spot-hover-control"
                  aria-hidden="true"
                  :style="{ left: `${spotPointer.x}px`, top: `${spotPointer.y}px` }"
                >
                  <img class="core-spot-hover-arrow" :src="arrowIcon" alt="" />
                </span>
              </div>
            </article>
          </div>
        </section>
        <section class="site-section photo-gallery-section" aria-labelledby="photo-gallery-title">
          <h2 id="photo-gallery-title" class="type-52 photo-gallery-title">美图欣赏</h2>
          <div class="photo-gallery-grid">
            <figure v-for="(photo, index) in galleryPhotos" :key="photo" class="photo-gallery-item">
              <img :src="photo" :alt="`风度村景观照片 ${index + 1}`" />
            </figure>
          </div>
        </section>
      </template>
      <template v-else-if="currentPage === '/test'">
        <div class="page-heading">
          <p class="eyebrow">DESIGN SYSTEM · TEST</p>
          <h1>测试</h1>
          <p class="placeholder-note">图文混排与色彩系统预览</p>
        </div>

        <section class="site-section section--auto type-demo" aria-labelledby="type-demo-title">
          <div class="type-demo-media">
            <img src="https://images.unsplash.com/photo-1528127269322-539801943592?auto=format&fit=crop&w=1400&q=85" alt="山川与道路风景" />
          </div>
          <div class="type-demo-copy">
            <p class="type-14 type-demo-kicker">ZUNYI · RED MEMORY</p>
            <h2 id="type-demo-title" class="type-36">在山河之间，重新认识一座城</h2>
            <p class="type-24 type-demo-lead">让每一次抵达，都成为理解遵义的入口。</p>
            <p class="type-16 type-demo-body">从历史现场到城市日常，沿着时间留下的线索慢慢行走。这里先保留内容结构，后续将替换为真实文案、图片与页面模块。</p>
          </div>
        </section>

        <section class="site-section section--auto color-system" aria-labelledby="color-system-title">
          <div class="section-heading">
            <p class="type-14 eyebrow">COLOR SYSTEM</p>
            <h2 id="color-system-title" class="type-36">颜色系统</h2>
          </div>
          <div v-for="scale in colorScales" :key="scale.name" class="color-scale">
            <div class="type-16 color-scale-name">{{ scale.name }}</div>
            <div class="swatch-row">
              <div v-for="(color, index) in scale.colors" :key="color" class="swatch-item">
                <span class="swatch" :style="{ backgroundColor: color }"></span>
                <span class="type-14 swatch-label">{{ (index + 1) * 10 }}</span>
              </div>
            </div>
          </div>
          <div class="semantic-row">
            <div v-for="color in semanticColors" :key="color.name" class="semantic-item">
              <span class="swatch" :style="{ backgroundColor: color.value }"></span>
              <span class="type-14">{{ color.name }}</span>
            </div>
          </div>
          <div class="usage-rules">
            <div class="section-heading">
              <p class="type-14 eyebrow">COLOR USAGE</p>
              <h3 class="type-24">颜色使用规则</h3>
            </div>
            <div class="usage-rule-list">
              <div v-for="rule in colorUsageRules" :key="rule.title" class="usage-rule">
                <strong class="type-36">{{ rule.value }}</strong>
                <div>
                  <h4 class="type-16">{{ rule.title }}</h4>
                  <p class="type-14">{{ rule.text }}</p>
                </div>
              </div>
            </div>
            <p class="type-14 text-rule-note">文本默认只使用主文本和次文本两个层级；只有在确有信息层次需要时，才使用弱文本层级。</p>
          </div>
        </section>
      </template>
      <template v-else>
        <div class="page-heading">
          <p class="eyebrow">RED STYLE · ZUNYI</p>
          <h1>{{ currentLabel }}</h1>
          <p class="placeholder-note">页面内容占位</p>
        </div>
        <div class="empty-canvas" aria-hidden="true"></div>
      </template>
    </main>

    <footer class="site-footer">
      <div class="footer-main">
        <a class="footer-brand" href="#/" @click="navigate('/')">
          <span class="brand-mark">红</span>
          <span><strong>红色风度</strong><small>遵义旅游官网</small></span>
        </a>
        <nav class="footer-nav" aria-label="底部导航">
          <a v-for="page in pages" :key="`footer-${page.path}`" :href="`#${page.path}`" @click="navigate(page.path)">
            {{ page.label }}
          </a>
        </nav>
      </div>
      <div class="footer-bottom">
        <span>© 2026 红色风度旅游官网</span>
        <span>隐私政策 · 使用条款</span>
      </div>
    </footer>
  </div>
</template>
