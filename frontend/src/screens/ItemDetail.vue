<script setup>
/* eslint-disable @typescript-eslint/no-unused-vars -- generated imports, matched generously */
import { computed } from "vue";
import { Icons } from "@/lib/icons";
import { brand } from "@/lib/brand";
import { useNavigate } from "@/lib/navigate";

const { IconFileText, IconArrowLeft, IconAlertCircle, IconCheckCircle } = Icons;
const navigate = useNavigate();

const item = {
  id: 'itm-4471',
  title: 'The Left Hand of Darkness',
  author: 'Ursula K. Le Guin',
  isbn: '978-0-441-47812-5',
  publisher: 'Ace Books',
  publication_year: '1969',
  subject: 'Science Fiction',
  shelf_location: 'Floor 2 — SF/LEG',
  availability_status: 'Available',
  description:
    'An envoy from an interstellar coalition is sent to the frozen world of Gethen, where inhabitants have no fixed gender. A landmark work of speculative fiction.',
  cover_image_key: null,
  updated_at: '12 Oct 2026'
}

const bibliographic = [
  { id: 'i-2-0-0', label: 'Author', value: item.author },
  { id: 'i-2-0-1', label: 'ISBN', value: item.isbn, numeric: true },
  { id: 'i-2-0-2', label: 'Publisher', value: item.publisher },
  { id: 'i-2-0-3', label: 'Publication year', value: item.publication_year, numeric: true },
  { id: 'i-2-0-4', label: 'Subject', value: item.subject },
  { id: 'i-2-0-5', label: 'Shelf location', value: item.shelf_location }
]

const hasCover = computed(() => Boolean(item.cover_image_key))
const isAvailable = computed(() => item.availability_status === 'Available')
</script>

<template>
  <div class="min-h-full bg-white pb-16" :style="{ color: brand.primaryColor, fontFamily: brand.fontBody }">
    <!-- Row 1 -->
    <div class="grid grid-cols-12 gap-4">
      <div class="col-span-12 sm:col-span-3">
        <button
          data-wf="b-0-0"
          type="button"
          @click="navigate('catalogue')"
          class="inline-flex items-center gap-2 px-2 py-2 -ml-2 text-[13px] font-medium uppercase tracking-[0.12em] underline underline-offset-4 decoration-2 hover:bg-neutral-100 focus:outline-none focus-visible:ring-2 focus-visible:ring-offset-2"
          :style="{ color: brand.primaryColor, borderRadius: brand.radius }"
        >
          <IconArrowLeft class="h-4 w-4" />
          Back to results
        </button>
      </div>
    </div>

    <!-- Row 2 -->
    <div class="mt-6 grid grid-cols-12 gap-8">
      <div data-wf="b-1-0" class="col-span-12 md:col-span-4">
        <div
          class="aspect-[2/3] w-full border-2 flex items-center justify-center overflow-hidden"
          :style="{ borderColor: brand.primaryColor, borderRadius: brand.radius }"
        >
          <img
            v-if="hasCover"
            :src="item.cover_image_key"
            :alt="'Cover of ' + item.title"
            class="h-full w-full object-cover"
          />
          <div v-else class="flex flex-col items-center gap-3 px-6 text-center">
            <IconFileText class="h-10 w-10" :style="{ color: brand.neutralColor }" />
            <p class="text-[12px] uppercase tracking-[0.14em]" :style="{ color: brand.neutralColor }">
              No cover uploaded
            </p>
            <p class="text-[12px] leading-5" :style="{ color: brand.neutralColor }">
              A placeholder is shown until library staff upload a cover image for this record.
            </p>
          </div>
        </div>
        <p class="mt-3 text-[12px] uppercase tracking-[0.14em]" :style="{ color: brand.neutralColor }">
          Cover image
        </p>
      </div>

      <div data-wf="b-1-1" class="col-span-12 md:col-span-8">
        <p class="text-[12px] uppercase tracking-[0.18em]" :style="{ color: brand.neutralColor }">
          Catalogue record · {{ item.id }}
        </p>
        <h1
          class="mt-3 text-[34px] leading-[1.1] font-bold tracking-tight"
          :style="{ fontFamily: brand.fontHeading, color: brand.primaryColor }"
        >
          {{ item.title }}
        </h1>
        <div class="mt-5 h-[2px] w-full" :style="{ backgroundColor: brand.primaryColor }"></div>
        <p class="mt-4 text-[14px] leading-6" :style="{ color: brand.neutralColor }">
          {{ item.author }} · {{ item.publisher }}, {{ item.publication_year }} · Record last updated {{ item.updated_at }}
        </p>
      </div>
    </div>

    <!-- Row 3 -->
    <div class="mt-10 grid grid-cols-12 gap-8">
      <div data-wf="b-2-0" class="col-span-12 md:col-span-8">
        <h2
          class="text-[13px] font-bold uppercase tracking-[0.16em]"
          :style="{ fontFamily: brand.fontHeading, color: brand.primaryColor }"
        >
          Bibliographic details
        </h2>
        <div class="mt-3 h-[2px] w-full" :style="{ backgroundColor: brand.primaryColor }"></div>
        <dl class="w-full">
          <div
            v-for="field in bibliographic"
            :key="field.id"
            :data-wf="field.id"
            class="grid grid-cols-12 gap-4 py-3 border-b"
            :style="{ borderColor: 'rgba(17,17,17,0.14)' }"
          >
            <dt
              class="col-span-5 sm:col-span-4 text-[12px] uppercase tracking-[0.14em] leading-5"
              :style="{ color: brand.neutralColor }"
            >
              {{ field.label }}
            </dt>
            <dd
              class="col-span-7 sm:col-span-8 text-[14px] leading-5 font-medium"
              :class="field.numeric ? 'tabular-nums' : ''"
              :style="{ color: brand.primaryColor }"
            >
              {{ field.value }}
            </dd>
          </div>
        </dl>
      </div>

      <div data-wf="b-2-1" class="col-span-12 md:col-span-4">
        <div
          class="border-2 p-5"
          :style="{ borderColor: brand.primaryColor, borderRadius: brand.radius }"
        >
          <p class="text-[12px] uppercase tracking-[0.16em]" :style="{ color: brand.neutralColor }">
            Availability
          </p>
          <div class="mt-3 flex items-center gap-2">
            <IconCheckCircle v-if="isAvailable" class="h-5 w-5" :style="{ color: brand.primaryColor }" />
            <IconAlertCircle v-else class="h-5 w-5" :style="{ color: brand.accentColor }" />
            <span
              class="text-[20px] font-bold tracking-tight"
              :style="{ fontFamily: brand.fontHeading, color: isAvailable ? brand.primaryColor : brand.accentColor }"
            >
              {{ item.availability_status }}
            </span>
          </div>
          <div class="mt-4 h-[2px] w-full" :style="{ backgroundColor: brand.primaryColor }"></div>
          <p class="mt-4 text-[13px] leading-5" :style="{ color: brand.neutralColor }">
            On the shelf at {{ item.shelf_location }}. Status is set by library staff and shown here as read-only.
          </p>
        </div>
      </div>
    </div>

    <!-- Row 4 -->
    <div class="mt-10 grid grid-cols-12 gap-8">
      <div data-wf="b-3-0" class="col-span-12">
        <h2
          class="text-[13px] font-bold uppercase tracking-[0.16em]"
          :style="{ fontFamily: brand.fontHeading, color: brand.primaryColor }"
        >
          Description
        </h2>
        <div class="mt-3 h-[2px] w-full" :style="{ backgroundColor: brand.primaryColor }"></div>
        <p class="mt-4 max-w-[68ch] text-[15px] leading-7" :style="{ color: brand.primaryColor }">
          {{ item.description }}
        </p>
      </div>
    </div>
  </div>
</template>
