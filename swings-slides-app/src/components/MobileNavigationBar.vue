<script setup lang="ts">
import { useUserLocationStore } from '../stores/userLocation';
import { inject } from 'vue';
import { useRouter } from 'vue-router';

const items = inject('items') as Array<{ label: string; icon: string; badge?: number; items?: any[] }> | undefined
const isMobile = inject('isMobile') as boolean | undefined

const navBgClass = 'bg-white dark:bg-black dark:text-white';
const navShadowClass = 'shadow-2xl dark:shadow-[0_-4px_24px_0_rgba(255,255,255,0.12)] shadow-[0_-4px_24px_0_rgba(0,0,0,0.12)] dark:shadow-2xl';

const userLocationStore = useUserLocationStore();
const router = useRouter();

const handlePlacesClick = () => {
  userLocationStore.fetchUserLocationWithWatch();
};
</script>

<template>
	<div v-if="isMobile" :class="navBgClass">
      <header class="fixed top-0 left-0 right-0 z-50 overflow-x-auto scrollbar-none" :class="`${navBgClass} ${navShadowClass} w-full`">
        <NavBarUserSection 
          avatar-class="ml-4 w-10 h-10 rounded-full object-cover aspect-square"
          color-mode-class=""
          search-class="w-full max-w-[80%]"
          :wrapperClass="`flex items-center justify-between p-4 w-full ${navBgClass}`"
        />
      </header>
      <nav class="fixed bottom-0 left-0 right-0 z-50" :class="`${navBgClass} ${navShadowClass} w-full`">
        <div class="flex flex-row items-center justify-between px-2 py-3 gap-1 overflow-x-auto whitespace-nowrap scrollbar-none" :class="navBgClass">
          <template v-for="item in items" :key="item.label">
            <RouterLink
              v-if="item.label.toLowerCase() !== 'places' && item.label.toLowerCase() !== 'create'"
              :to="{ name: (item.label || '').toLowerCase() }"
              class="flex flex-col items-center justify-center min-w-0 px-3"
              style="text-decoration: none;"
              active-class="text-blue-600"
            >
              <i :class="[item.icon, 'text-2xl mb-1', $route.name === (item.label || '').toLowerCase() ? 'text-blue-600' : 'text-black dark:text-white']" />
              <span class="text-xs truncate" :class="[$route.name === (item.label || '').toLowerCase() ? 'text-blue-600' : 'text-black', navBgClass]">{{ item.label }}</span>
            </RouterLink>
            <button
              v-else-if="item.label.toLowerCase() === 'places'"
              class="flex flex-col items-center justify-center min-w-0 px-3 opacity-100 cursor-pointer focus:outline-none"
              @click="handlePlacesClick"
              :aria-busy="userLocationStore.isLocationLoading"
              :aria-label="'Find places near me'"
            >
              <i :class="[item.icon, 'text-2xl mb-1 text-black dark:text-white']" />
              <span class="text-xs text-black truncate" :class="navBgClass">{{ item.label }}</span>
            </button>
            <button
              v-else-if="item.label.toLowerCase() === 'create'"
              class="flex flex-col items-center justify-center min-w-0 px-3 opacity-100 cursor-pointer focus:outline-none"
              @click="router.push('/create-park')"
              :aria-label="'Create park or event'"
            >
              <i :class="[item.icon, 'text-2xl mb-1 text-black dark:text-white']" />
              <span class="text-xs text-black truncate" :class="navBgClass">{{ item.label }}</span>
            </button>
          </template>
        </div>
      </nav>
    </div>
</template>