<template>
  <div class="fixed z-[-1] bg-[#F2F2F2] w-full h-[100vh]" />
  <NuxtPage />

  <MenuOverlay
    :class="[userStore.isMenuOverlay ? 'max-h-[100vh] transition-all duration-200 ease-in visible' : 'max-h-0 transition-all duration-200 ease-out invisible']" />
</template>

<script setup>
import { useUserStore } from "./stores/user";

const userStore = useUserStore();

const route = useRoute();

const windowWidth = ref(process.client ? window.innerWidth : '');

onMounted(() => {
  userStore.isLoading = false;
  window.addEventListener("resize", () => {
    windowWidth.value = window.innerWidth;
  });
});

watch(windowWidth, () => {
  if (windowWidth.value >= 767) {
    userStore.isMenuOverlay = false;
  }
});

watch(route.fullPath, () => {
  userStore.isLoading = true;
});
</script>