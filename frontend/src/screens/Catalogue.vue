<script setup>
/* eslint-disable @typescript-eslint/no-unused-vars -- generated imports, matched generously */
import { ref, computed, onUnmounted, nextTick } from "vue";
import { Button, Input, Label } from "@/lib/ui";
import { Icons } from "@/lib/icons";
import { brand } from "@/lib/brand";
import { useNavigate } from "@/lib/navigate";

const { IconSearch, IconCheck, IconX, IconChevronRight, IconFileText, IconClock, IconFilter, IconAlertCircle, IconCheckCircle } = Icons;
const navigate = useNavigate();

const SCREEN_LABEL = "Public catalogue · no login needed";

const mix = (colour, percent) => `color-mix(in srgb, ${colour} ${percent}%, transparent)`;
const rule = `2px solid ${brand.primaryColor}`;
const hair = `1px solid ${mix(brand.primaryColor, 16)}`;

const ITEMS = [
  {
    id: "ITM-00417",
    wf: "i-4-0-0",
    title: "The Left Hand of Darkness",
    author: "Ursula K. Le Guin",
    subject: "Science Fiction",
    publication_year: 1969,
    shelf_location: "SF LEG 4-12",
    availability_status: "Available",
    cover_image_key: "covers/itm-00417.jpg",
    description: "Two envoys cross the frozen ocean of the Gobrin Ice on the winter world of Gethen.",
  },
  {
    id: "ITM-00193",
    wf: "i-4-0-1",
    title: "The Old Man and the Sea",
    author: "Ernest Hemingway",
    subject: "Literary Fiction",
    publication_year: 1952,
    shelf_location: "FIC HEM 2-03",
    availability_status: "On loan",
    cover_image_key: "covers/itm-00193.jpg",
    description: "Santiago rows far into the open ocean and hooks a marlin longer than his skiff.",
  },
  {
    id: "ITM-01122",
    wf: "i-4-0-2",
    title: "Ocean Circulation in a Warming World",
    author: "M. Hartley",
    subject: "Oceanography",
    publication_year: 2021,
    shelf_location: "551.46 HAR 1-08",
    availability_status: "Available",
    cover_image_key: "",
    description: "Measurements of the Atlantic overturning circulation from 2004 to 2020.",
  },
  {
    id: "ITM-00876",
    wf: "i-4-0-3",
    title: "Blue Mind",
    author: "Wallace J. Nichols",
    subject: "Popular Science",
    publication_year: 2014,
    shelf_location: "REF 152.4 NIC",
    availability_status: "Reference only",
    cover_image_key: "covers/itm-00876.jpg",
    description: "Why time spent near the ocean makes us calmer, healthier and better connected.",
  },
  {
    id: "ITM-00418",
    wf: "",
    title: "The Dispossessed",
    author: "Ursula K. Le Guin",
    subject: "Science Fiction",
    publication_year: 1974,
    shelf_location: "SF LEG 4-13",
    availability_status: "Available",
    cover_image_key: "covers/itm-00418.jpg",
    description: "The physicist Shevek leaves anarchist Anarres for the propertied world of Urras.",
  },
  {
    id: "ITM-00419",
    wf: "",
    title: "A Wizard of Earthsea",
    author: "Ursula K. Le Guin",
    subject: "Science Fiction",
    publication_year: 1968,
    shelf_location: "SF LEG 4-14",
    availability_status: "On loan",
    cover_image_key: "covers/itm-00419.jpg",
    description: "Ged sails the archipelago and names the shadow he loosed as a student on Roke.",
  },
  {
    id: "ITM-00655",
    wf: "",
    title: "Tide and Time: Atlantic Fisheries 1890–1990",
    author: "Nuala Fitzgerald",
    subject: "Maritime History",
    publication_year: 1998,
    shelf_location: "639.2 FIT 3-02",
    availability_status: "Available",
    cover_image_key: "",
    description: "A century of trawling out of Killybegs, told through harbour ledgers and crew lists.",
  },
  {
    id: "ITM-00921",
    wf: "",
    title: "Silent Spring",
    author: "Rachel Carson",
    subject: "Environmental Science",
    publication_year: 1962,
    shelf_location: "363.73 CAR 2-11",
    availability_status: "Available",
    cover_image_key: "covers/itm-00921.jpg",
    description: "The 1962 study of pesticide use that started modern environmental regulation.",
  },
  {
    id: "ITM-01044",
    wf: "",
    title: "The Dawn of Everything",
    author: "David Graeber & David Wengrow",
    subject: "Anthropology",
    publication_year: 2021,
    shelf_location: "930.1 GRA 5-04",
    availability_status: "On loan",
    cover_image_key: "covers/itm-01044.jpg",
    description: "A new history of early humanity, from seasonal cities to the first bureaucracies.",
  },
  {
    id: "ITM-00502",
    wf: "",
    title: "Station Eleven",
    author: "Emily St. John Mandel",
    subject: "Science Fiction",
    publication_year: 2014,
    shelf_location: "SF MAN 4-21",
    availability_status: "Reference only",
    cover_image_key: "covers/itm-00502.jpg",
    description: "A travelling symphony performs Shakespeare twenty years after a collapse.",
  },
];

const FACETS = [
  { id: "i-2-0-0", field: "author", label: "Author: Ursula K. Le Guin", value: "Ursula K. Le Guin" },
  { id: "i-2-0-1", field: "subject", label: "Subject: Science Fiction", value: "Science Fiction" },
  { id: "i-2-0-2", field: "availability_status", label: "Availability: Available", value: "Available" },
];

const MAX_QUERY = 60;

const query = ref("ocean");
const submitted = ref("ocean");
const activeFacets = ref([]);
const status = ref("ready");
const revealed = ref(true);
const searchInput = ref(null);

let timer = null;

const matchesQuery = (item, q) => {
  if (!q) return true;
  const needle = q.toLowerCase();
  return `${item.title} ${item.author} ${item.subject} ${item.description}`.toLowerCase().includes(needle);
};

const matchesFacets = (item) =>
  FACETS.filter((f) => activeFacets.value.includes(f.id)).every((f) => item[f.field] === f.value);

const results = computed(() => ITEMS.filter((i) => matchesQuery(i, submitted.value) && matchesFacets(i)));

const activeCount = computed(() => activeFacets.value.length);

const summary = computed(() => {
  const n = results.value.length;
  const noun = n === 1 ? "item" : "items";
  const filterNote = activeCount.value
    ? ` with ${activeCount.value} filter${activeCount.value === 1 ? "" : "s"} applied`
    : "";
  if (!submitted.value) {
    return `Browsing all ${n} ${noun}${filterNote}. Showing the most recently catalogued first.`;
  }
  return `${n} ${noun} match “${submitted.value}”${filterNote}. Showing the most relevant first.`;
});

const isActive = (id) => activeFacets.value.includes(id);

function reveal() {
  revealed.value = false;
  nextTick(() => {
    revealed.value = true;
  });
}

function runSearch() {
  const q = query.value.trim();
  if (q.length > MAX_QUERY) {
    status.value = "error";
    return;
  }
  if (timer) clearTimeout(timer);
  status.value = "loading";
  revealed.value = false;
  timer = setTimeout(() => {
    submitted.value = q;
    status.value = "ready";
    nextTick(() => {
      revealed.value = true;
    });
  }, 420);
}

function toggleFacet(id) {
  activeFacets.value = isActive(id) ? activeFacets.value.filter((f) => f !== id) : [...activeFacets.value, id];
  if (status.value === "ready") reveal();
}

function clearFilters() {
  if (!activeFacets.value.length) return;
  activeFacets.value = [];
  reveal();
}

function clearSearchAndFilters() {
  activeFacets.value = [];
  query.value = "";
  submitted.value = "";
  status.value = "ready";
  reveal();
  nextTick(() => searchInput.value && searchInput.value.focus && searchInput.value.focus());
}

function initials(title) {
  return title
    .replace(/[^A-Za-z0-9 ]/g, " ")
    .split(" ")
    .filter((w) => w && !["the", "a", "an", "of", "and", "in"].includes(w.toLowerCase()))
    .slice(0, 2)
    .map((w) => w[0].toUpperCase())
    .join("");
}

const chipStyle = (on) => ({
  border: rule,
  borderRadius: brand.radius,
  backgroundColor: on ? brand.primaryColor : "#ffffff",
  color: on ? "#ffffff" : brand.primaryColor,
  outlineColor: brand.accentColor,
});

onUnmounted(() => {
  if (timer) clearTimeout(timer);
});
</script>

<template>
  <div class="bg-white" :style="{ color: brand.primaryColor, fontFamily: brand.fontBody }">
    <!-- Row 1 -->
    <div data-wf="b-0-0" class="flex items-end justify-between gap-6 pb-4" :style="{ borderBottom: rule }">
      <div>
        <p class="text-[11px] font-bold uppercase tracking-[0.18em]" :style="{ color: brand.neutralColor }">
          {{ SCREEN_LABEL }}
        </p>
        <h1 class="mt-1 text-3xl font-bold tracking-tight" :style="{ fontFamily: brand.fontHeading }">
          Search the library catalogue
        </h1>
      </div>
      <p
        class="hidden text-right text-[11px] font-semibold uppercase leading-5 tracking-[0.14em] tabular-nums sm:block"
        :style="{ color: brand.neutralColor }"
      >
        9,412 holdings<br />Index synced 09:40, 4 Nov 2026
      </p>
    </div>

    <!-- Row 2 -->
    <form class="grid grid-cols-12 gap-3 py-4" @submit.prevent="runSearch">
      <div class="col-span-12 md:col-span-9">
        <Label for="cat-q" class="sr-only">Search by title, author, subject or description</Label>
        <div class="relative">
          <IconSearch
            class="pointer-events-none absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2"
            aria-hidden="true"
            :style="{ color: brand.neutralColor }"
          />
          <Input
            id="cat-q"
            ref="searchInput"
            v-model="query"
            data-wf="b-1-0"
            type="search"
            autocomplete="off"
            placeholder="Search by title, author, subject or description"
            class="h-11 w-full bg-white pl-9 pr-3 text-sm outline-none focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2"
            :style="{ border: rule, borderRadius: brand.radius, outlineColor: brand.accentColor }"
          />
        </div>
        <p class="mt-1.5 text-[11px] font-semibold tabular-nums" :style="{ color: brand.neutralColor }">
          Keyword search runs across title, author, subject and description. Up to {{ MAX_QUERY }} characters.
        </p>
      </div>
      <div class="col-span-12 md:col-span-3">
        <Button
          data-wf="b-1-1"
          type="submit"
          :disabled="status === 'loading'"
          class="h-11 w-full text-xs font-bold uppercase tracking-[0.14em] text-white transition-opacity disabled:opacity-40 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2"
          :style="{ backgroundColor: brand.primaryColor, borderRadius: brand.radius, outlineColor: brand.accentColor }"
        >
          <span v-if="status === 'loading'">Searching…</span>
          <span v-else>Search</span>
        </Button>
      </div>
    </form>

    <!-- Row 3 -->
    <div data-wf="b-2-0" class="flex flex-wrap items-center gap-2 pb-4" :style="{ borderBottom: hair }">
      <span class="mr-1 text-[11px] font-bold uppercase tracking-[0.18em]" :style="{ color: brand.neutralColor }">
        Filters
      </span>
      <button
        v-for="f in FACETS"
        :key="f.id"
        :data-wf="f.id"
        type="button"
        :aria-pressed="isActive(f.id)"
        class="inline-flex h-9 items-center gap-2 px-3 text-[11px] font-bold uppercase tracking-[0.14em] transition-colors focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2"
        :style="chipStyle(isActive(f.id))"
        @click="toggleFacet(f.id)"
      >
        <IconFilter class="h-3.5 w-3.5" aria-hidden="true" />
        <span>{{ f.label }}</span>
        <IconCheck v-if="isActive(f.id)" class="h-3.5 w-3.5" aria-hidden="true" />
      </button>
      <button
        data-wf="i-2-0-3"
        type="button"
        :disabled="!activeCount"
        class="ml-auto inline-flex h-9 items-center gap-2 px-3 text-[11px] font-bold uppercase tracking-[0.14em] transition-opacity disabled:opacity-30 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2"
        :style="{ border: rule, borderRadius: brand.radius, outlineColor: brand.accentColor }"
        @click="clearFilters"
      >
        <IconX class="h-3.5 w-3.5" aria-hidden="true" />
        <span>Clear all filters</span>
      </button>
    </div>

    <!-- Row 4 -->
    <p
      data-wf="b-3-0"
      aria-live="polite"
      class="py-3 text-sm font-semibold tabular-nums"
      :style="{ borderBottom: hair }"
    >
      <template v-if="status === 'loading'">Searching the catalogue index for “{{ query.trim() }}”…</template>
      <template v-else-if="status === 'error'">Search not run — the keyword is too long for the index.</template>
      <template v-else>{{ summary }}</template>
    </p>

    <!-- Error state -->
    <div
      v-if="status === 'error'"
      class="my-4 flex flex-wrap items-start gap-3 p-4"
      :style="{ border: `2px solid ${brand.accentColor}`, borderRadius: brand.radius }"
    >
      <IconAlertCircle class="mt-0.5 h-5 w-5 shrink-0" aria-hidden="true" :style="{ color: brand.accentColor }" />
      <div class="min-w-[16rem] flex-1">
        <p class="text-sm font-bold uppercase tracking-[0.14em]" :style="{ color: brand.accentColor }">
          Keyword too long
        </p>
        <p class="mt-1 max-w-[62ch] text-sm leading-6 tabular-nums">
          The search index accepts {{ MAX_QUERY }} characters and this keyword is {{ query.trim().length }}. Your text is
          still in the box — trim it to the words that matter and search again.
        </p>
      </div>
    </div>

    <!-- Loading state -->
    <div v-else-if="status === 'loading'" class="pt-2" aria-hidden="true">
      <div
        v-for="n in 4"
        :key="n"
        class="flex items-center gap-4 py-3 motion-safe:animate-pulse motion-reduce:animate-none"
        :style="{ borderBottom: hair }"
      >
        <span class="h-[60px] w-11 shrink-0" :style="{ border: hair, backgroundColor: mix(brand.primaryColor, 6) }" />
        <span class="flex-1">
          <span class="block h-3 w-1/3" :style="{ backgroundColor: mix(brand.primaryColor, 14) }" />
          <span class="mt-2 block h-2.5 w-1/4" :style="{ backgroundColor: mix(brand.primaryColor, 8) }" />
        </span>
        <span class="h-2.5 w-20" :style="{ backgroundColor: mix(brand.primaryColor, 8) }" />
      </div>
    </div>

    <!-- Row 5 -->
    <section
      v-else-if="results.length"
      data-wf="b-4-0"
      class="pt-4 transition-all duration-200 motion-reduce:transition-none"
      :style="{ opacity: revealed ? 1 : 0, transform: revealed ? 'none' : 'translateY(4px)' }"
    >
      <h2 class="pb-2 text-[11px] font-bold uppercase tracking-[0.18em]" :style="{ color: brand.neutralColor }">
        Results
      </h2>
      <ul :style="{ borderTop: rule }">
        <li v-for="item in results" :key="item.id" :style="{ borderBottom: hair }">
          <button
            type="button"
            :data-wf="item.wf || undefined"
            class="flex w-full items-center gap-4 py-3 text-left transition-colors hover:bg-black/[0.03] focus-visible:outline focus-visible:outline-2 focus-visible:-outline-offset-2"
            :style="{ outlineColor: brand.accentColor }"
            @click="navigate('item-detail')"
          >
            <span
              class="flex h-[60px] w-11 shrink-0 flex-col items-center justify-center"
              :style="{ border: rule, backgroundColor: item.cover_image_key ? brand.primaryColor : '#ffffff' }"
            >
              <template v-if="item.cover_image_key">
                <span class="text-[13px] font-bold tracking-tight text-white">{{ initials(item.title) }}</span>
                <span class="mt-0.5 text-[7px] font-bold uppercase tracking-[0.12em] text-white/70 tabular-nums">
                  {{ item.publication_year }}
                </span>
                <span class="sr-only">Cover image on file</span>
              </template>
              <template v-else>
                <IconFileText class="h-4 w-4" aria-hidden="true" :style="{ color: brand.neutralColor }" />
                <span class="mt-1 text-[7px] font-bold uppercase tracking-[0.1em]" :style="{ color: brand.neutralColor }">
                  No cover
                </span>
              </template>
            </span>

            <span class="min-w-0 flex-1">
              <span class="block truncate text-sm font-semibold">{{ item.title }}</span>
              <span class="mt-0.5 block truncate text-[13px]" :style="{ color: brand.neutralColor }">
                {{ item.author }}
              </span>
              <span
                class="mt-1 block text-[11px] font-bold uppercase tracking-[0.14em] tabular-nums"
                :style="{ color: brand.neutralColor }"
              >
                {{ item.subject }} · {{ item.publication_year }} · Shelf {{ item.shelf_location }}
              </span>
            </span>

            <span
              class="inline-flex w-[8.5rem] shrink-0 items-center justify-end gap-1.5 text-[11px] font-bold uppercase tracking-[0.14em]"
              :style="{ color: item.availability_status === 'Available' ? brand.primaryColor : brand.accentColor }"
            >
              <IconCheckCircle v-if="item.availability_status === 'Available'" class="h-3.5 w-3.5" aria-hidden="true" />
              <IconClock v-else-if="item.availability_status === 'On loan'" class="h-3.5 w-3.5" aria-hidden="true" />
              <IconAlertCircle v-else class="h-3.5 w-3.5" aria-hidden="true" />
              {{ item.availability_status }}
            </span>
            <IconChevronRight class="h-4 w-4 shrink-0" aria-hidden="true" :style="{ color: brand.neutralColor }" />
          </button>
        </li>
      </ul>
      <p class="pt-3 text-[11px] font-semibold uppercase tracking-[0.14em] tabular-nums" :style="{ color: brand.neutralColor }">
        Showing {{ results.length }} of {{ results.length }} matches · Availability is set by library staff
      </p>
    </section>

    <!-- Row 6 -->
    <section
      v-else
      data-wf="b-5-0"
      class="mt-4 px-6 py-12 text-center transition-opacity duration-200 motion-reduce:transition-none"
      :style="{ border: rule, borderRadius: brand.radius, opacity: revealed ? 1 : 0 }"
    >
      <p class="text-[11px] font-bold uppercase tracking-[0.18em]" :style="{ color: brand.neutralColor }">
        No matches
      </p>
      <h2 class="mx-auto mt-2 max-w-[48ch] text-lg font-bold tracking-tight" :style="{ fontFamily: brand.fontHeading }">
        <template v-if="submitted">Nothing matched “{{ submitted }}”.</template>
        <template v-else>Nothing in the catalogue matches these filters.</template>
      </h2>
      <p class="mx-auto mt-2 max-w-[56ch] text-sm leading-6" :style="{ color: brand.neutralColor }">
        Try another keyword or clear your filters. Spelling and partial words both work — “hemingw” finds Hemingway.
      </p>
      <button
        type="button"
        class="mt-5 inline-flex h-9 items-center gap-2 px-4 text-[11px] font-bold uppercase tracking-[0.14em] focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2"
        :style="{ border: rule, borderRadius: brand.radius, outlineColor: brand.accentColor }"
        @click="clearSearchAndFilters"
      >
        <IconX class="h-3.5 w-3.5" aria-hidden="true" />
        Clear search and filters
      </button>
    </section>
  </div>
</template>
