<script setup>
/* eslint-disable @typescript-eslint/no-unused-vars -- generated imports, matched generously */
import { ref, computed } from "vue";
import { Select, Table, THead, TBody, TR, TH, TD, Empty } from "@/lib/ui";
import { Icons } from "@/lib/icons";
import { brand } from "@/lib/brand";
import { useNavigate } from "@/lib/navigate";

const { IconPlus, IconSearch, IconTrash, IconFilter, IconCheckCircle } = Icons;
const navigate = useNavigate();

const RECORD_TOTAL = 1482

const query = ref('')
const subjectFilter = ref('all')
const availabilityFilter = ref('all')
const pendingDelete = ref(null)
const lastDeleted = ref('')

const records = ref([
  { id: 'itm-4412', title: 'The Old Man and the Sea', author: 'Ernest Hemingway', isbn: '978-0684801223', subject: 'Fiction', shelf_location: 'FIC HEM 3-B', availability_status: 'Available', updated_at: '2026-10-28' },
  { id: 'itm-4408', title: 'A Brief History of Time', author: 'Stephen Hawking', isbn: '978-0553380163', subject: 'Science', shelf_location: '523.1 HAW 7-A', availability_status: 'On loan', updated_at: '2026-10-27' },
  { id: 'itm-4401', title: 'The Oxford English Dictionary, Vol. II', author: 'J. A. Simpson (ed.)', isbn: '978-0198611868', subject: 'Reference', shelf_location: 'REF 423 OED 1-A', availability_status: 'Reference only', updated_at: '2026-10-24' },
  { id: 'itm-4396', title: 'Beloved', author: 'Toni Morrison', isbn: '978-1400033416', subject: 'Fiction', shelf_location: 'FIC MOR 4-C', availability_status: 'Available', updated_at: '2026-10-22' },
  { id: 'itm-4390', title: 'Silent Spring', author: 'Rachel Carson', isbn: '978-0618249060', subject: 'Science', shelf_location: '363.73 CAR 7-D', availability_status: 'Available', updated_at: '2026-10-21' },
  { id: 'itm-4387', title: 'The Wright Brothers', author: 'David McCullough', isbn: '978-1476728742', subject: 'History', shelf_location: '629.13 MCC 6-B', availability_status: 'On loan', updated_at: '2026-10-19' },
  { id: 'itm-4381', title: 'Goodnight Moon', author: 'Margaret Wise Brown', isbn: '978-0060775858', subject: "Children's", shelf_location: 'JUV BRO 2-A', availability_status: 'Available', updated_at: '2026-10-18' },
  { id: 'itm-4375', title: 'Thinking, Fast and Slow', author: 'Daniel Kahneman', isbn: '978-0374533557', subject: 'Psychology', shelf_location: '153.42 KAH 8-C', availability_status: 'On loan', updated_at: '2026-10-15' },
  { id: 'itm-4370', title: 'The Code Breaker', author: 'Walter Isaacson', isbn: '978-1982115852', subject: 'Science', shelf_location: '576.5 ISA 7-B', availability_status: 'Available', updated_at: '2026-10-14' },
  { id: 'itm-4366', title: 'Pachinko', author: 'Min Jin Lee', isbn: '978-1455563937', subject: 'Fiction', shelf_location: 'FIC LEE 4-A', availability_status: 'Available', updated_at: '2026-10-12' },
  { id: 'itm-4359', title: 'Atlas of the World, 28th Edition', author: 'National Geographic', isbn: '978-1426222467', subject: 'Reference', shelf_location: 'REF 912 NAT 1-C', availability_status: 'Reference only', updated_at: '2026-10-09' },
  { id: 'itm-4352', title: 'The Warmth of Other Suns', author: 'Isabel Wilkerson', isbn: '978-0679763888', subject: 'History', shelf_location: '307.76 WIL 6-A', availability_status: 'Available', updated_at: '2026-10-07' },
  { id: 'itm-4347', title: 'Where the Crawdads Sing', author: 'Delia Owens', isbn: '978-0735219090', subject: 'Fiction', shelf_location: 'FIC OWE 4-D', availability_status: 'On loan', updated_at: '2026-10-05' },
  { id: 'itm-4341', title: 'Sapiens: A Brief History of Humankind', author: 'Yuval Noah Harari', isbn: '978-0062316097', subject: 'History', shelf_location: '909 HAR 6-C', availability_status: 'Available', updated_at: '2026-10-02' },
  { id: 'itm-4336', title: 'The Very Hungry Caterpillar', author: 'Eric Carle', isbn: '978-0399226908', subject: "Children's", shelf_location: 'JUV CAR 2-B', availability_status: 'Available', updated_at: '2026-09-30' }
])

const subjectOptions = [
  { value: 'all', label: 'Subject: All' },
  { value: 'Fiction', label: 'Subject: Fiction' },
  { value: 'Science', label: 'Subject: Science' },
  { value: 'History', label: 'Subject: History' },
  { value: 'Reference', label: 'Subject: Reference' },
  { value: 'Psychology', label: 'Subject: Psychology' },
  { value: "Children's", label: "Subject: Children's" }
]

const availabilityOptions = [
  { value: 'all', label: 'Availability: All' },
  { value: 'Available', label: 'Availability: Available' },
  { value: 'On loan', label: 'Availability: On loan' },
  { value: 'Reference only', label: 'Availability: Reference only' }
]

const filtered = computed(() => {
  const q = query.value.trim().toLowerCase()
  return records.value.filter((r) => {
    const matchesQuery =
      !q ||
      r.title.toLowerCase().includes(q) ||
      r.author.toLowerCase().includes(q) ||
      r.isbn.toLowerCase().includes(q)
    const matchesSubject = subjectFilter.value === 'all' || r.subject === subjectFilter.value
    const matchesAvailability =
      availabilityFilter.value === 'all' || r.availability_status === availabilityFilter.value
    return matchesQuery && matchesSubject && matchesAvailability
  })
})

const filtersActive = computed(
  () => query.value.trim() !== '' || subjectFilter.value !== 'all' || availabilityFilter.value !== 'all'
)

const deletedCount = computed(() => RECORD_TOTAL - records.value.length - (RECORD_TOTAL - 1482))

const totalRecords = computed(() => RECORD_TOTAL - (15 - records.value.length))

const countLine = computed(() => {
  if (filtersActive.value) {
    const n = filtered.value.length
    return n === 0
      ? 'No records match the current search and filters.'
      : `${n} matching ${n === 1 ? 'record' : 'records'} of ${totalRecords.value.toLocaleString()} in the catalogue. Showing 1–${n}.`
  }
  return `${totalRecords.value.toLocaleString()} records in the catalogue. Showing 1–20.`
})

function clearFilters() {
  query.value = ''
  subjectFilter.value = 'all'
  availabilityFilter.value = 'all'
}

function askDelete(record) {
  pendingDelete.value = record
}

function cancelDelete() {
  pendingDelete.value = null
}

function confirmDelete() {
  if (!pendingDelete.value) return
  const title = pendingDelete.value.title
  records.value = records.value.filter((r) => r.id !== pendingDelete.value.id)
  lastDeleted.value = title
  pendingDelete.value = null
}

function isException(status) {
  return status !== 'Available'
}
</script>

<template>
  <div
    class="min-h-full bg-white px-6 py-6 text-[#111111]"
    :style="{ backgroundColor: brand.backgroundColor, fontFamily: brand.fontBody }"
  >
    <!-- Row 1 -->
    <div class="grid grid-cols-12 items-end gap-4 border-b-2 border-[#111111] pb-4">
      <div data-wf="b-0-0" class="col-span-12 md:col-span-8">
        <p class="text-[12px] font-bold uppercase tracking-[0.18em] text-[#5c5c5c]">Staff area</p>
        <h1
          class="mt-1 text-[32px] font-bold leading-[1.15] tracking-[-0.02em]"
          :style="{ fontFamily: brand.fontHeading }"
        >
          Catalogue management
        </h1>
      </div>

      <div class="col-span-6 md:col-span-2">
        <button
          data-wf="b-0-1"
          type="button"
          class="flex h-11 w-full items-center justify-center gap-2 border-2 border-[#111111] bg-[#111111] px-3 text-[14px] font-bold uppercase tracking-[0.08em] text-white transition-colors hover:bg-white hover:text-[#111111] focus:outline-none focus:ring-2 focus:ring-orange-700 focus:ring-offset-2 active:translate-y-px"
          :style="{ borderRadius: brand.radius }"
          @click="navigate('manage-item')"
        >
          <IconPlus class="h-4 w-4" />
          <span>New item</span>
        </button>
      </div>

      <div class="col-span-6 md:col-span-2">
        <button
          data-wf="b-0-2"
          type="button"
          class="h-11 w-full border-2 border-[#111111] bg-white px-3 text-[14px] font-bold uppercase tracking-[0.08em] text-[#111111] transition-colors hover:bg-[#111111] hover:text-white focus:outline-none focus:ring-2 focus:ring-orange-700 focus:ring-offset-2 active:translate-y-px"
          :style="{ borderRadius: brand.radius }"
          @click="navigate('staff-login')"
        >
          Log out
        </button>
      </div>
    </div>

    <!-- Row 2 -->
    <div class="mt-5 grid grid-cols-12 gap-4">
      <div data-wf="b-1-0" class="col-span-12 lg:col-span-7">
        <label
          for="record-search"
          class="block text-[12px] font-bold uppercase tracking-[0.18em] text-[#5c5c5c]"
        >
          Search records
        </label>
        <form class="mt-2 flex items-center border-2 border-[#111111] bg-white focus-within:ring-2 focus-within:ring-orange-700 focus-within:ring-offset-2"
          :style="{ borderRadius: brand.radius }"
          @submit.prevent
        >
          <span class="pl-3 text-[#5c5c5c]"><IconSearch class="h-4 w-4" /></span>
          <input
            id="record-search"
            v-model="query"
            type="search"
            placeholder="Find a record by title, author or ISBN"
            class="h-11 w-full bg-transparent px-3 text-[14px] text-[#111111] placeholder:text-[#5c5c5c] focus:outline-none"
          />
        </form>
      </div>

      <div data-wf="b-1-1" class="col-span-12 lg:col-span-5">
        <div class="flex items-center justify-between">
          <span class="text-[12px] font-bold uppercase tracking-[0.18em] text-[#5c5c5c]">Filter</span>
          <button
            v-if="filtersActive"
            type="button"
            class="text-[12px] font-bold uppercase tracking-[0.12em] underline underline-offset-4 focus:outline-none focus:ring-2 focus:ring-orange-700 focus:ring-offset-2"
            :style="{ color: brand.accentColor }"
            @click="clearFilters"
          >
            Clear all
          </button>
        </div>
        <div class="mt-2 grid grid-cols-2 gap-4">
          <div class="flex items-center border-2 border-[#111111] bg-white" :style="{ borderRadius: brand.radius }">
            <span class="pl-3 text-[#5c5c5c]"><IconFilter class="h-4 w-4" /></span>
            <Select
              data-wf="i-1-1-0"
              v-model="subjectFilter"
              :options="subjectOptions"
              aria-label="Filter by subject"
              class="h-11 w-full border-0 bg-transparent px-2 text-[14px] font-medium text-[#111111] focus:outline-none focus:ring-2 focus:ring-orange-700"
            />
          </div>
          <div class="flex items-center border-2 border-[#111111] bg-white" :style="{ borderRadius: brand.radius }">
            <span class="pl-3 text-[#5c5c5c]"><IconFilter class="h-4 w-4" /></span>
            <Select
              data-wf="i-1-1-1"
              v-model="availabilityFilter"
              :options="availabilityOptions"
              aria-label="Filter by availability"
              class="h-11 w-full border-0 bg-transparent px-2 text-[14px] font-medium text-[#111111] focus:outline-none focus:ring-2 focus:ring-orange-700"
            />
          </div>
        </div>
      </div>
    </div>

    <!-- Row 3 -->
    <div data-wf="b-2-0" class="mt-5 border-t-2 border-[#111111] pt-3">
      <p class="text-[13px] font-medium tabular-nums text-[#5c5c5c]" aria-live="polite">
        {{ countLine }}
      </p>
    </div>

    <!-- Row 4 -->
    <section data-wf="b-3-0" class="mt-4">
      <div class="flex items-baseline justify-between">
        <h2
          class="text-[18px] font-bold tracking-[-0.01em]"
          :style="{ fontFamily: brand.fontHeading }"
        >
          Item records
        </h2>
        <span class="text-[12px] font-bold uppercase tracking-[0.18em] text-[#5c5c5c]">
          Select a row to edit
        </span>
      </div>

      <p
        v-if="lastDeleted"
        class="mt-3 flex items-center gap-2 border-2 px-3 py-2 text-[13px] font-medium"
        :style="{ borderColor: brand.accentColor, color: brand.accentColor, borderRadius: brand.radius }"
        role="status"
      >
        <IconCheckCircle class="h-4 w-4 shrink-0" />
        <span>&ldquo;{{ lastDeleted }}&rdquo; was deleted. It no longer appears in public search or browse.</span>
      </p>

      <div class="mt-3 border-2 border-[#111111]" :style="{ borderRadius: brand.radius }">
        <Table class="w-full border-collapse text-left text-[13px]">
          <THead>
            <TR class="border-b-2 border-[#111111]">
              <TH data-wf="i-3-0-0" class="px-3 py-2 text-[11px] font-bold uppercase tracking-[0.14em] text-[#111111]">Title</TH>
              <TH data-wf="i-3-0-1" class="px-3 py-2 text-[11px] font-bold uppercase tracking-[0.14em] text-[#111111]">Author</TH>
              <TH data-wf="i-3-0-2" class="px-3 py-2 text-[11px] font-bold uppercase tracking-[0.14em] text-[#111111]">ISBN</TH>
              <TH data-wf="i-3-0-3" class="px-3 py-2 text-[11px] font-bold uppercase tracking-[0.14em] text-[#111111]">Subject</TH>
              <TH data-wf="i-3-0-4" class="px-3 py-2 text-[11px] font-bold uppercase tracking-[0.14em] text-[#111111]">Shelf location</TH>
              <TH data-wf="i-3-0-5" class="px-3 py-2 text-[11px] font-bold uppercase tracking-[0.14em] text-[#111111]">Availability</TH>
              <TH data-wf="i-3-0-6" class="px-3 py-2 text-[11px] font-bold uppercase tracking-[0.14em] text-[#111111]">Updated</TH>
              <TH class="px-3 py-2 text-right text-[11px] font-bold uppercase tracking-[0.14em] text-[#111111]">Action</TH>
            </TR>
          </THead>
          <TBody>
            <TR
              v-for="record in filtered"
              :key="record.id"
              tabindex="0"
              class="cursor-pointer border-b border-[#111111]/15 align-middle transition-colors last:border-b-0 hover:bg-[#111111]/[0.04] focus:outline-none focus:ring-2 focus:ring-inset focus:ring-orange-700"
              @click="navigate('manage-item')"
              @keydown.enter="navigate('manage-item')"
            >
              <TD class="px-3 py-2 font-bold">
                <span
                  class="mr-2 inline-block h-3 w-[2px] translate-y-[2px]"
                  :style="{ backgroundColor: isException(record.availability_status) ? brand.accentColor : 'transparent' }"
                  aria-hidden="true"
                ></span>{{ record.title }}
              </TD>
              <TD class="px-3 py-2 text-[#111111]">{{ record.author }}</TD>
              <TD class="px-3 py-2 tabular-nums text-[#5c5c5c]">{{ record.isbn }}</TD>
              <TD class="px-3 py-2 text-[#111111]">{{ record.subject }}</TD>
              <TD class="px-3 py-2 tabular-nums text-[#111111]">{{ record.shelf_location }}</TD>
              <TD class="px-3 py-2">
                <span
                  class="inline-block border-2 px-2 py-[1px] text-[11px] font-bold uppercase tracking-[0.1em]"
                  :style="{
                    borderColor: isException(record.availability_status) ? brand.accentColor : brand.primaryColor,
                    color: isException(record.availability_status) ? brand.accentColor : brand.primaryColor,
                    borderRadius: brand.radius
                  }"
                >
                  {{ record.availability_status }}
                </span>
              </TD>
              <TD class="px-3 py-2 tabular-nums text-[#5c5c5c]">{{ record.updated_at }}</TD>
              <TD class="px-3 py-2 text-right">
                <button
                  type="button"
                  class="inline-flex h-9 items-center gap-1 border-2 border-[#111111] bg-white px-2 text-[11px] font-bold uppercase tracking-[0.1em] text-[#111111] transition-colors hover:text-white focus:outline-none focus:ring-2 focus:ring-orange-700 focus:ring-offset-2"
                  :style="{ borderRadius: brand.radius }"
                  :aria-label="'Delete ' + record.title"
                  @click.stop="askDelete(record)"
                  @mouseenter="$event.currentTarget.style.backgroundColor = brand.accentColor; $event.currentTarget.style.borderColor = brand.accentColor"
                  @mouseleave="$event.currentTarget.style.backgroundColor = '#ffffff'; $event.currentTarget.style.borderColor = brand.primaryColor"
                >
                  <IconTrash class="h-3.5 w-3.5" />
                  <span>Delete</span>
                </button>
              </TD>
            </TR>
          </TBody>
        </Table>

        <div v-if="filtered.length === 0" class="px-4 py-10">
          <Empty
            title="No records match this search"
            description="Nothing in the catalogue matches that keyword and filter combination. Clear the filters or try a different title, author or ISBN."
          >
            <template #action>
              <button
                type="button"
                class="h-11 border-2 border-[#111111] bg-[#111111] px-4 text-[13px] font-bold uppercase tracking-[0.1em] text-white transition-colors hover:bg-white hover:text-[#111111] focus:outline-none focus:ring-2 focus:ring-orange-700 focus:ring-offset-2"
                :style="{ borderRadius: brand.radius }"
                @click="clearFilters"
              >
                Clear search and filters
              </button>
            </template>
          </Empty>
        </div>
      </div>
    </section>

    <!-- Row 5 -->
    <div
      v-if="pendingDelete"
      data-wf="b-4-0"
      class="fixed inset-0 z-50 flex items-center justify-center bg-[#111111]/60 px-6"
      role="dialog"
      aria-modal="true"
      aria-labelledby="delete-dialog-title"
      @keydown.esc="cancelDelete"
    >
      <div
        class="w-full max-w-lg border-2 border-[#111111] bg-white p-6 shadow-2xl"
        :style="{ borderRadius: brand.radius }"
      >
        <p class="text-[12px] font-bold uppercase tracking-[0.18em]" :style="{ color: brand.accentColor }">
          Confirm deletion
        </p>
        <h2
          id="delete-dialog-title"
          class="mt-2 text-[20px] font-bold leading-[1.35] tracking-[-0.01em]"
          :style="{ fontFamily: brand.fontHeading }"
        >
          Delete &ldquo;{{ pendingDelete.title }}&rdquo;? This removes it from public search and browse.
        </h2>
        <p class="mt-3 text-[13px] leading-[1.6] text-[#5c5c5c]">
          Record {{ pendingDelete.id }} &middot; {{ pendingDelete.author }} &middot; shelf {{ pendingDelete.shelf_location }}.
          This cannot be undone from the dashboard.
        </p>

        <div class="mt-6 flex flex-col gap-3 border-t-2 border-[#111111] pt-4 sm:flex-row sm:justify-end">
          <button
            data-wf="i-4-0-1"
            type="button"
            class="h-11 border-2 border-[#111111] bg-white px-4 text-[13px] font-bold uppercase tracking-[0.1em] text-[#111111] transition-colors hover:bg-[#111111] hover:text-white focus:outline-none focus:ring-2 focus:ring-orange-700 focus:ring-offset-2"
            :style="{ borderRadius: brand.radius }"
            @click="cancelDelete"
          >
            Cancel
          </button>
          <button
            data-wf="i-4-0-0"
            type="button"
            class="h-11 border-2 px-4 text-[13px] font-bold uppercase tracking-[0.1em] text-white transition-opacity hover:opacity-85 focus:outline-none focus:ring-2 focus:ring-orange-700 focus:ring-offset-2"
            :style="{
              backgroundColor: brand.accentColor,
              borderColor: brand.accentColor,
              borderRadius: brand.radius
            }"
            @click="confirmDelete"
          >
            Delete record
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
