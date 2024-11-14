<script setup lang="ts">
import { IconDragDrop } from '@tabler/icons-vue';
import { useHead } from '@vueuse/head';
import { computed } from 'vue';
import Draggable from 'vuedraggable';
import ToolCard from '../components/ToolCard.vue';
import { useToolStore } from '@/tools/tools.store';

const toolStore = useToolStore();

useHead({
  title: 'IT Tools - Essential Developer Toolkit',
  meta: [
    { 
      name: 'description', 
      content: 'Free collection of essential developer tools including formatters, converters, generators, and validators. Built for developers, by developers.'
    },
      { 
      name: 'keywords',
      content: 'developer tools, online tools, web development, IT tools, code formatter, converter, generator'
    },
    {
      property: 'og:title', 
      content: 'IT Tools - Essential Developer Toolkit'
    },
    {
      property: 'og:description',
      content: 'Free collection of essential developer tools including formatters, converters, generators, and validators. Built for developers, by developers.'
    },
    { property: 'og:type', content: 'website' },
    { name: 'twitter:card', content: 'summary_large_image' },
  ],
});
const { t } = useI18n();

const favoriteTools = computed(() => toolStore.favoriteTools);

// Update favorite tools order when drag is finished
function onUpdateFavoriteTools() {
  toolStore.updateFavoriteTools(favoriteTools.value); // Update the store with the new order
}
</script>

<template>
  <div class="home-container">
    <!-- Hero Section -->
    <section class="hero-section">
      <h1 class="hero-title">{{ $t('IT Tools - Essential Developer Toolkit') }}</h1>
      <p class="hero-subtitle">{{ $t('home.subtitle') }}</p>
    </section>

    <!-- Tools Grid -->
    <div class="tools-container">
      <!-- Favorite Tools Section -->
      <transition name="fade-slide" appear>
        <section v-if="toolStore.favoriteTools.length > 0" class="tools-section">
          <h2 class="section-title">
            {{ $t('home.categories.favoriteTools') }}
            <c-tooltip :tooltip="$t('home.categories.favoritesDndToolTip')">
              <n-icon :component="IconDragDrop" class="drag-icon" />
            </c-tooltip>
          </h2>
          <Draggable
            :list="favoriteTools"
            class="tools-grid"
            ghost-class="ghost-card"
            item-key="name"
            @end="onUpdateFavoriteTools"
          >
            <template #item="{ element: tool }">
              <ToolCard :tool="tool" class="tool-card" />
            </template>
          </Draggable>
        </section>
      </transition>

      <!-- New Tools Section -->
      <section v-if="toolStore.newTools.length > 0" class="tools-section">
        <h2 class="section-title">{{ t('home.categories.newestTools') }}</h2>
        <div class="tools-grid">
          <ToolCard 
            v-for="tool in toolStore.newTools" 
            :key="tool.name" 
            :tool="tool" 
            class="tool-card"
          />
        </div>
      </section>

      <!-- All Tools Section -->
      <section class="tools-section">
        <h2 class="section-title">{{ $t('home.categories.allTools') }}</h2>
        <div class="tools-grid">
          <ToolCard 
            v-for="tool in toolStore.tools" 
            :key="tool.name" 
            :tool="tool" 
            class="tool-card"
          />
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped lang="less">
.home-container {
  @apply max-w-7xl mx-auto px-4 py-8;
}

.hero-section {
  @apply text-center mb-12;
}

.hero-title {
  @apply text-4xl md:text-5xl font-bold mb-4 bg-gradient-to-r from-primary to-primary-light bg-clip-text text-transparent;
}

.hero-subtitle {
  @apply text-lg text-neutral-600 dark:text-neutral-400;
}

.tools-section {
  @apply mb-12;
}

.section-title {
  @apply text-2xl font-medium mb-6 flex items-center gap-2 text-neutral-800 dark:text-neutral-200;
}

.tools-grid {
  @apply grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 sm:gap-6;
}

.tool-card {
  @apply transition-all duration-200 hover:scale-102 hover:shadow-lg;
}

.ghost-card {
  @apply opacity-60 bg-neutral-100 dark:bg-neutral-800 border-2 border-dashed border-primary rounded-lg shadow-md;
}

.drag-icon {
  @apply text-neutral-500 w-5 h-5;
}

// Animations
.fade-slide-enter-active,
.fade-slide-leave-active {
  @apply transition-all duration-300 ease-out;
}

.fade-slide-enter-from,
.fade-slide-leave-to {
  @apply opacity-0 transform translate-y-4;
}
</style>
