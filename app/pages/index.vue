<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'
import gsap from 'gsap'
import { ScrollToPlugin } from 'gsap/ScrollToPlugin'

gsap.registerPlugin(ScrollToPlugin)

const verses = [
  { text: '清雪漫空凝夜色，', type: 'verse' },
  { text: '浮生独向冷中明。', type: 'verse' },
  { text: '—— 清生', type: 'author' }
]

const sections = ref<HTMLElement[]>([])
let isScrolling = false
let currentIndex = 0

const temp = document.createElement('div')
temp.style.height = 'var(--ui-header-height)'
document.body.appendChild(temp)
const headerHeight = parseFloat(getComputedStyle(temp).height) || 0
document.body.removeChild(temp)
// 滚动处理函数
const scrollToIndex = (index: number) => {
  if (index < 0 || index >= 3 || isScrolling) return

  isScrolling = true
  currentIndex = index

  gsap.to(window, {
    duration: 1,
    scrollTo: { y: index * window.innerHeight - headerHeight, autoKill: false },
    ease: "power2.inOut",
    onComplete: () => {
      isScrolling = false
    }
  })
}

const handleWheel = (e: WheelEvent) => {
  e.preventDefault() // 阻止原生滚动
  if (isScrolling) return

  if (e.deltaY > 0) {
    scrollToIndex(currentIndex + 1)
  } else {
    scrollToIndex(currentIndex - 1)
  }
}

onMounted(() => {
  // 禁用原生平滑滚动
  document.documentElement.style.scrollBehavior = 'auto'
  window.addEventListener('wheel', handleWheel, { passive: false })
})

onUnmounted(() => {
  window.removeEventListener('wheel', handleWheel)
})
</script>
<template>
  <div>
    <UPageSection id="" class="h-[calc(100vh-var(--ui-header-height))] overflow-hidden relative ">
      <!-- 背景雪花 -->
      <SnowfallBg color="E6E6FA" class="absolute inset-0 pointer-events-none" :min-radius="0.2" :max-radius="5"
        :speed="0.5" />

      <div>
        <!-- 液态玻璃：统一清透紫白磨砂 -->
        <LiquidGlass
          container-class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 z-50 w-[700px] max-w-[90vw]"
          :radius="24" :border="0.05" :lightness="96" blend="screen" xChannel="B" yChannel="R" :alpha="0.98" :blur="9"
          :rOffset="0" :gOffset="0" :bOffset="8" :scale="-150" :frost="0.08"
          class="border border-white/20 shadow-lg shadow-blue-500/10">
          <div
            class="flex min-h-16 w-full items-center justify-center px-12 py-6 text-purple-100/90 font-serif text-lg">
            人生的意义是什么，奉献 or 享乐？
          </div>
        </LiquidGlass>
      </div>
      <!-- 诗句容器：靠右居中 -->
      <div class="absolute right-12 top-1/2 -translate-y-1/2 z-10 
        flex flex-col items-center gap-16
        [writing-mode:vertical-rl]">

        <div v-for="(item, index) in verses" :key="index">
          <CardSpotlight :gradient-size="180" gradient-color="rgba(139,92,246,0.6)" :gradient-opacity="1"
            class="bg-transparent border-none dark:bg-transparent overflow-visible "
            slot-class="flex items-center justify-center">

            <template #default>
              <p :class="[
                item.type === 'verse'
                  ? 'text-4xl text-purple-200/60 tracking-[0.6em] font-medium transition-colors duration-700'
                  : 'text-2xl text-indigo-200/30 tracking-[0.4em] font-light mt-8'
              ]" class="relative z-20 hover:text-white font-serif 
          whitespace-nowrap overflow-visible text-nowrap">
                {{ item.text }}
              </p>
            </template>
          </CardSpotlight>
        </div>
      </div>
    </UPageSection>

    <UPageSection id="" class="h-[calc(100vh-var(--ui-header-height))] overflow-hidden relative border-2 ">
      <div class="flex items-center justify-center h-full  text-4xl">第二屏内容</div>
    </UPageSection>

    <UPageSection id="" class="h-[calc(100vh-var(--ui-header-height))] overflow-hidden relative ">
      <div class="flex items-center justify-center h-full  text-4xl">第三屏内容</div>
    </UPageSection>
  </div>
</template>
