<script setup lang="ts">
useHead({
  meta: [
    { name: 'viewport', content: 'width=device-width, initial-scale=1' }
  ],
  link: [
    { rel: 'icon', href: '/favicon.ico' }
  ],
  htmlAttrs: {
    lang: 'en'
  }
})

const title = 'Nuxt Starter Template'
const description = 'A production-ready starter template powered by Nuxt UI. Build beautiful, accessible, and performant applications in minutes, not hours.'
const tabs = ["人生观", "技能", "展示", ];
const activeTab = ref<string>("人生观");
useSeoMeta({
  title,
  description,
  ogTitle: title,
  ogDescription: description,
  ogImage: 'https://ui.nuxt.com/assets/templates/nuxt/starter-light.png',
  twitterCard: 'summary_large_image'
})
</script>

<template>
  <UApp>
    <UHeader class="sticky top-0 z-50 bg-white/80 dark:bg-neutral-900/80 backdrop-blur">
      <template #title>
        <SparklesText text="CHING SHENG" :colors="{ first: '#9E7AFF', second: '#FE8BBB' }" :sparkles-count="10"
          class="my-8"></SparklesText>
      </template>
      
        <MorphingTabs :tabs="tabs" :active-tab="activeTab" @update:active-tab="activeTab = $event"></MorphingTabs>
      
    </UHeader>

    <UMain>
      <NuxtPage />
    </UMain>

    <!-- <USeparator icon="i-simple-icons-nuxtdotjs" />

    <UFooter>
      <template #left>
        <p class="text-sm text-muted">
          Built with Nuxt UI • © {{ new Date().getFullYear() }}
        </p>
      </template>

      <template #right>
        <UButton
          to="https://github.com/nuxt-ui-templates/starter"
          target="_blank"
          icon="i-simple-icons-github"
          aria-label="GitHub"
          color="neutral"
          variant="ghost"
        />
      </template>
    </UFooter> -->
  </UApp>
</template>
