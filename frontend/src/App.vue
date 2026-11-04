<script lang="ts">
import type { RouteRecordRaw } from "vue-router";

import Catalogue from "@/screens/Catalogue.vue";
import ItemDetail from "@/screens/ItemDetail.vue";
import StaffLogin from "@/screens/StaffLogin.vue";
import Manage from "@/screens/Manage.vue";
import ManageItem from "@/screens/ManageItem.vue";

// Screens marked bare (shown before sign-in, or hidden from the menu) render on their own.
export const routes: RouteRecordRaw[] = [
  { path: "/catalogue", component: Catalogue },
  { path: "/item-detail", component: ItemDetail },
  { path: "/staff-login", component: StaffLogin, meta: { bare: true } },
  { path: "/manage", component: Manage },
  { path: "/manage-item", component: ManageItem },
  { path: "/:pathMatch(.*)*", redirect: "/catalogue" },
];
</script>

<script setup lang="ts">
import { computed } from "vue";
import { RouterLink, RouterView, useRoute } from "vue-router";
import { Icons } from "@/lib/icons";

const { IconSearch, IconUser } = Icons;
const route = useRoute();
const bare = computed(() => route.meta.bare === true);

function linkClass(path: string): string {
  const active = path === "/" ? route.path === "/" : route.path.startsWith(path);
  return [
    "flex items-center gap-2 rounded-[var(--brand-radius)] px-3 py-2 text-sm font-medium transition-colors",
    active ? "bg-[var(--brand-hover)] text-[var(--brand-fg)]" : "text-[var(--brand-fg-muted)]",
  ].join(" ");
}
</script>

<template>
  <RouterView v-if="bare" />
  <div v-else class="flex min-h-screen flex-col">
    <header
      class="border-b"
      style="background-color: var(--brand-surface); border-color: var(--brand-border)"
    >
      <div class="flex items-center gap-6 px-6 py-2">
        <p class="shrink-0 text-sm font-semibold" style="font-family: var(--brand-font-heading)">
          {{ "Library Catalogue" }}
        </p>
        <nav aria-label="App menu" class="flex flex-wrap items-center gap-1">
          <RouterLink to="/catalogue" :class="linkClass('/catalogue')">
            <IconSearch :size="16" />
            {{ "Search catalogue" }}
          </RouterLink>
          <RouterLink to="/manage" :class="linkClass('/manage')">
            <IconUser :size="16" />
            {{ "Staff area" }}
          </RouterLink>
        </nav>
      </div>
    </header>
    <main class="flex-1 overflow-auto">
      <RouterView />
    </main>
  </div>
</template>
