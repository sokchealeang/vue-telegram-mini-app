<script setup lang="ts">
import { computed, onMounted, ref } from "vue";
import { routes } from "@/router";
import AppPage from "@/components/AppPage.vue";
import AppLink from "@/components/AppLink.vue";
import { isTMA } from "@telegram-apps/sdk-vue";

const isTelegram = ref(false);

const nonIndexRoutes = computed(() => routes.filter((r) => !!r.meta?.title));
const userAgent = computed(() => navigator.userAgent);

async function checkTelegram() {
  isTelegram.value = await isTMA();
}

onMounted(checkTelegram);
</script>

<template>
  <AppPage title="Home Page" :back="false">
    <p>{{ `${isTelegram}` ? "From Telegram" : "From Web" }}</p>
    <p>User Agent: {{ userAgent }}</p>
    <p>
      This page is a home page in this boilerplate. You can use the links below
      to visit other pages with their own functionality.
    </p>
    <ul class="index-page__links">
      <li
        v-for="route in nonIndexRoutes"
        :key="route.name"
        class="index-page__link-item"
      >
        <AppLink class="index-page__link" :to="{ name: route.name }">
          <i v-if="route.meta?.icon" class="index-page__link-icon">
            <component :is="route.meta.icon" />
          </i>
          {{ route.meta!.title }}
        </AppLink>
      </li>
    </ul>
  </AppPage>
</template>

<style scoped>
.index-page__links {
  list-style: none;
  padding-left: 0;
}

.index-page__link {
  font-weight: bold;
  display: inline-flex;
  gap: 5px;
}

.index-page__link-item + .index-page__link-item {
  margin-top: 10px;
}

.index-page__link-icon {
  width: 20px;
  display: block;
}

.index-page__link-icon svg {
  display: block;
}
</style>
