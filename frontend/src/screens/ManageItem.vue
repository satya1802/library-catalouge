<script setup>
/* eslint-disable @typescript-eslint/no-unused-vars -- generated imports, matched generously */
import { ref, reactive, computed } from "vue";
import { Input, Textarea, Label, Select } from "@/lib/ui";
import { Icons } from "@/lib/icons";
import { brand } from "@/lib/brand";
import { useNavigate } from "@/lib/navigate";

const { IconCheck, IconX, IconFileText, IconTrash, IconUpload, IconArrowLeft, IconAlertCircle, IconCheckCircle } = Icons;
const navigate = useNavigate();

const ink = brand.primaryColor
const accent = brand.accentColor

const availabilityOptions = [
  { value: 'Available', label: 'Available' },
  { value: 'On loan', label: 'On loan' },
  { value: 'Reference only', label: 'Reference only' }
]

const original = {
  id: 'itm_0481',
  title: 'The Making of the Atomic Bomb',
  author: 'Richard Rhodes',
  isbn: '9781451677614',
  publisher: 'Simon & Schuster',
  publication_year: '1986',
  subject: 'History of Science',
  description:
    'Pulitzer Prize-winning narrative history of the Manhattan Project, from Leo Szilard’s 1933 insight to Hiroshima. 25th anniversary edition, 886 pages, includes notes and index.',
  shelf_location: 'HS-204 / Bay 3, Shelf B',
  availability_status: 'On loan',
  cover_image_key: 'covers/9781451677614-rhodes.jpg'
}

const form = reactive({ ...original })

const coverName = ref('9781451677614-rhodes.jpg')
const coverUrl = ref('')
const fileInput = ref(null)

const touched = ref(false)
const fileError = ref('')
const statusNote = ref('Last saved 2026-10-29 by h.okonkwo@eastport.lib')
const confirmOpen = ref(false)
const saving = ref(false)

const titleError = computed(() => (touched.value && !form.title.trim() ? 'Title is required.' : ''))
const authorError = computed(() => (touched.value && !form.author.trim() ? 'Author is required.' : ''))
const subjectError = computed(() =>
  touched.value && !form.subject.trim() ? 'Subject / category is required.' : ''
)

const problems = computed(() =>
  [titleError.value, authorError.value, subjectError.value, fileError.value].filter(Boolean)
)

function pickFile() {
  if (fileInput.value) fileInput.value.click()
}

function onFile(event) {
  const file = event.target.files && event.target.files[0]
  if (!file) return
  const name = file.name.toLowerCase()
  const ok = name.endsWith('.jpg') || name.endsWith('.jpeg') || name.endsWith('.png')
  if (!ok) {
    fileError.value = 'That file is not a supported image — the cover was not attached.'
    event.target.value = ''
    return
  }
  fileError.value = ''
  coverName.value = file.name
  form.cover_image_key = 'covers/' + file.name
  if (typeof URL !== 'undefined' && URL.createObjectURL) {
    coverUrl.value = URL.createObjectURL(file)
  }
  statusNote.value = 'New cover staged for upload to S3. Save the record to store it.'
  event.target.value = ''
}

function removeCover() {
  coverName.value = ''
  coverUrl.value = ''
  form.cover_image_key = ''
  fileError.value = ''
  statusNote.value = 'Cover removed. The public catalogue will show a placeholder.'
}

function saveRecord() {
  touched.value = true
  if (problems.value.length) {
    statusNote.value = 'Record not saved — correct the fields marked below.'
    return
  }
  saving.value = true
  statusNote.value = 'Saving record and reindexing…'
  navigate('manage')
}

function cancelEdits() {
  Object.assign(form, original)
  coverName.value = '9781451677614-rhodes.jpg'
  coverUrl.value = ''
  touched.value = false
  fileError.value = ''
  statusNote.value = 'Edits discarded. The record is back to its last saved state.'
}

function confirmDelete() {
  confirmOpen.value = false
  navigate('manage')
}
</script>

<template>
  <div
    class="min-h-full bg-white px-6 py-6 md:px-10"
    :style="{ color: ink, fontFamily: brand.fontBody }"
  >
    <!-- Row 1 -->
    <div class="grid grid-cols-12">
      <div class="col-span-12 md:col-span-4">
        <button
          data-wf="b-0-0"
          type="button"
          class="inline-flex min-h-[44px] items-center gap-2 border-b-2 border-transparent text-[13px] font-bold uppercase tracking-[0.12em] hover:border-current focus:outline-none focus:ring-2 focus:ring-offset-2"
          :style="{ color: ink, '--tw-ring-color': ink }"
          @click="navigate('manage')"
        >
          <IconArrowLeft class="h-4 w-4" />
          Back to catalogue management
        </button>
      </div>
    </div>

    <!-- Row 2 -->
    <div class="mt-4 grid grid-cols-12 border-b-2 pb-4" :style="{ borderColor: ink }">
      <div class="col-span-12" data-wf="b-1-0">
        <h1
          class="text-[32px] font-bold leading-tight tracking-[-0.02em] md:text-[36px]"
          :style="{ fontFamily: brand.fontHeading }"
        >
          Edit item record
        </h1>
        <p class="mt-2 text-[13px]" :style="{ color: brand.neutralColor }">
          Record {{ original.id }} &middot; ISBN {{ original.isbn }} &middot; {{ statusNote }}
        </p>
      </div>
    </div>

    <!-- Row 3 -->
    <div class="mt-6 grid grid-cols-12 gap-6">
      <!-- b-2-0 Item details form -->
      <form
        data-wf="b-2-0"
        class="col-span-12 border-2 md:col-span-8"
        :style="{ borderColor: ink }"
        @submit.prevent="saveRecord"
      >
        <div class="border-b-2 px-4 py-3" :style="{ borderColor: ink }">
          <h2 class="text-[13px] font-bold uppercase tracking-[0.16em]">Item details</h2>
        </div>

        <div class="grid grid-cols-12 gap-4 px-4 py-4">
          <div class="col-span-12">
            <Label
              for="f-title"
              class="mb-1 block text-[11px] font-bold uppercase tracking-[0.16em]"
              >Title <span :style="{ color: accent }">(required)</span></Label
            >
            <Input
              id="f-title"
              data-wf="i-2-0-0"
              v-model="form.title"
              placeholder="The Making of the Atomic Bomb"
              class="w-full border-2 bg-white px-3 py-2 text-[14px] focus:outline-none focus:ring-2 focus:ring-offset-1"
              :style="{ borderColor: titleError ? accent : ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
            />
            <p v-if="titleError" class="mt-1 text-[12px] font-bold" :style="{ color: accent }">
              {{ titleError }}
            </p>
          </div>

          <div class="col-span-12 md:col-span-6">
            <Label
              for="f-author"
              class="mb-1 block text-[11px] font-bold uppercase tracking-[0.16em]"
              >Author <span :style="{ color: accent }">(required)</span></Label
            >
            <Input
              id="f-author"
              data-wf="i-2-0-1"
              v-model="form.author"
              placeholder="Richard Rhodes"
              class="w-full border-2 bg-white px-3 py-2 text-[14px] focus:outline-none focus:ring-2 focus:ring-offset-1"
              :style="{ borderColor: authorError ? accent : ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
            />
            <p v-if="authorError" class="mt-1 text-[12px] font-bold" :style="{ color: accent }">
              {{ authorError }}
            </p>
          </div>

          <div class="col-span-12 md:col-span-6">
            <Label for="f-isbn" class="mb-1 block text-[11px] font-bold uppercase tracking-[0.16em]"
              >ISBN</Label
            >
            <Input
              id="f-isbn"
              data-wf="i-2-0-2"
              v-model="form.isbn"
              placeholder="9781451677614"
              class="w-full border-2 bg-white px-3 py-2 text-[14px] tabular-nums focus:outline-none focus:ring-2 focus:ring-offset-1"
              :style="{ borderColor: ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
            />
          </div>

          <div class="col-span-12 md:col-span-8">
            <Label
              for="f-publisher"
              class="mb-1 block text-[11px] font-bold uppercase tracking-[0.16em]"
              >Publisher</Label
            >
            <Input
              id="f-publisher"
              data-wf="i-2-0-3"
              v-model="form.publisher"
              placeholder="Simon & Schuster"
              class="w-full border-2 bg-white px-3 py-2 text-[14px] focus:outline-none focus:ring-2 focus:ring-offset-1"
              :style="{ borderColor: ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
            />
          </div>

          <div class="col-span-12 md:col-span-4">
            <Label for="f-year" class="mb-1 block text-[11px] font-bold uppercase tracking-[0.16em]"
              >Publication year</Label
            >
            <Input
              id="f-year"
              data-wf="i-2-0-4"
              v-model="form.publication_year"
              placeholder="1986"
              class="w-full border-2 bg-white px-3 py-2 text-[14px] tabular-nums focus:outline-none focus:ring-2 focus:ring-offset-1"
              :style="{ borderColor: ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
            />
          </div>

          <div class="col-span-12 md:col-span-6">
            <Label
              for="f-subject"
              class="mb-1 block text-[11px] font-bold uppercase tracking-[0.16em]"
              >Subject / category <span :style="{ color: accent }">(required)</span></Label
            >
            <Input
              id="f-subject"
              data-wf="i-2-0-5"
              v-model="form.subject"
              placeholder="History of Science"
              class="w-full border-2 bg-white px-3 py-2 text-[14px] focus:outline-none focus:ring-2 focus:ring-offset-1"
              :style="{ borderColor: subjectError ? accent : ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
            />
            <p v-if="subjectError" class="mt-1 text-[12px] font-bold" :style="{ color: accent }">
              {{ subjectError }}
            </p>
          </div>

          <div class="col-span-12 md:col-span-6">
            <Label
              for="f-shelf"
              class="mb-1 block text-[11px] font-bold uppercase tracking-[0.16em]"
              >Shelf location</Label
            >
            <Input
              id="f-shelf"
              data-wf="i-2-0-7"
              v-model="form.shelf_location"
              placeholder="HS-204 / Bay 3, Shelf B"
              class="w-full border-2 bg-white px-3 py-2 text-[14px] focus:outline-none focus:ring-2 focus:ring-offset-1"
              :style="{ borderColor: ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
            />
          </div>

          <div class="col-span-12">
            <Label
              for="f-description"
              class="mb-1 block text-[11px] font-bold uppercase tracking-[0.16em]"
              >Description</Label
            >
            <Textarea
              id="f-description"
              data-wf="i-2-0-6"
              v-model="form.description"
              rows="4"
              placeholder="Summary, edition notes, pagination"
              class="w-full border-2 bg-white px-3 py-2 text-[14px] leading-relaxed focus:outline-none focus:ring-2 focus:ring-offset-1"
              :style="{ borderColor: ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
            />
          </div>

          <div class="col-span-12 md:col-span-6">
            <Label
              for="f-availability"
              class="mb-1 block text-[11px] font-bold uppercase tracking-[0.16em]"
              >Availability status</Label
            >
            <Select
              id="f-availability"
              data-wf="i-2-0-8"
              v-model="form.availability_status"
              :options="availabilityOptions"
              class="w-full border-2 bg-white px-3 py-2 text-[14px] focus:outline-none focus:ring-2 focus:ring-offset-1"
              :style="{ borderColor: ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
            />
            <p class="mt-1 text-[12px]" :style="{ color: brand.neutralColor }">
              Shown read-only to the public and used as a search filter.
            </p>
          </div>
        </div>
      </form>

      <!-- b-2-1 Cover image -->
      <section
        data-wf="b-2-1"
        class="col-span-12 border-2 md:col-span-4"
        :style="{ borderColor: ink }"
      >
        <div class="border-b-2 px-4 py-3" :style="{ borderColor: ink }">
          <h2 class="text-[13px] font-bold uppercase tracking-[0.16em]">Cover image</h2>
        </div>

        <div class="px-4 py-4">
          <div
            data-wf="i-2-1-0"
            class="flex aspect-[2/3] w-full items-center justify-center border-2"
            :style="{ borderColor: ink }"
          >
            <img
              v-if="coverUrl"
              :src="coverUrl"
              alt="Selected cover for The Making of the Atomic Bomb"
              class="h-full w-full object-cover"
            />
            <div
              v-else-if="coverName"
              class="flex h-full w-full flex-col items-center justify-center gap-2 px-3 text-center"
            >
              <IconFileText class="h-8 w-8" />
              <span class="text-[12px] font-bold uppercase tracking-[0.12em]">Cover on file</span>
              <span class="break-all text-[12px]" :style="{ color: brand.neutralColor }">{{ coverName }}</span>
            </div>
            <div v-else class="flex h-full w-full flex-col items-center justify-center gap-2 px-3 text-center">
              <IconFileText class="h-8 w-8" :style="{ color: brand.neutralColor }" />
              <span class="text-[12px] font-bold uppercase tracking-[0.12em]">No cover</span>
              <span class="text-[12px]" :style="{ color: brand.neutralColor }">
                A placeholder is shown in the public catalogue.
              </span>
            </div>
          </div>

          <input
            ref="fileInput"
            type="file"
            accept=".jpg,.jpeg,.png,image/jpeg,image/png"
            class="sr-only"
            @change="onFile"
          />

          <button
            data-wf="i-2-1-1"
            type="button"
            class="mt-4 inline-flex min-h-[44px] w-full items-center justify-center gap-2 border-2 px-3 text-[13px] font-bold uppercase tracking-[0.12em] text-white hover:opacity-90 focus:outline-none focus:ring-2 focus:ring-offset-2"
            :style="{ backgroundColor: ink, borderColor: ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
            @click="pickFile"
          >
            <IconUpload class="h-4 w-4" />
            Upload new cover (JPG or PNG)
          </button>

          <button
            data-wf="i-2-1-2"
            type="button"
            :disabled="!coverName && !coverUrl"
            class="mt-2 inline-flex min-h-[44px] w-full items-center justify-center gap-2 border-2 bg-white px-3 text-[13px] font-bold uppercase tracking-[0.12em] hover:bg-neutral-100 focus:outline-none focus:ring-2 focus:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-40"
            :style="{ color: ink, borderColor: ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
            @click="removeCover"
          >
            <IconX class="h-4 w-4" />
            Remove cover
          </button>

          <p class="mt-3 text-[12px] leading-relaxed" :style="{ color: brand.neutralColor }">
            Stored in Amazon S3 as
            <span class="font-bold">{{ form.cover_image_key || '— none —' }}</span>.
          </p>
        </div>
      </section>
    </div>

    <!-- Row 4 -->
    <div class="mt-6 grid grid-cols-12">
      <div
        data-wf="b-3-0"
        class="col-span-12 border-2 px-4 py-3"
        :style="{ borderColor: problems.length ? accent : ink }"
        role="status"
        aria-live="polite"
      >
        <div v-if="problems.length" class="flex items-start gap-2">
          <IconAlertCircle class="mt-0.5 h-4 w-4 shrink-0" :style="{ color: accent }" />
          <div>
            <p class="text-[12px] font-bold uppercase tracking-[0.16em]" :style="{ color: accent }">
              Record not saved
            </p>
            <ul class="mt-1 space-y-1 text-[13px]">
              <li v-for="problem in problems" :key="problem">{{ problem }}</li>
            </ul>
          </div>
        </div>
        <div v-else class="flex items-start gap-2">
          <IconCheckCircle class="mt-0.5 h-4 w-4 shrink-0" />
          <p class="text-[13px]">
            Title, author and subject / category are required. Covers must be JPG or PNG.
            <span :style="{ color: brand.neutralColor }">{{ statusNote }}</span>
          </p>
        </div>
      </div>
    </div>

    <!-- Row 5 -->
    <div class="mt-6 grid grid-cols-12 gap-4">
      <div class="col-span-12 md:col-span-4">
        <button
          data-wf="b-4-0"
          type="button"
          :disabled="saving"
          class="inline-flex min-h-[44px] w-full items-center justify-center gap-2 border-2 px-4 text-[13px] font-bold uppercase tracking-[0.12em] text-white hover:opacity-90 focus:outline-none focus:ring-2 focus:ring-offset-2 disabled:opacity-60"
          :style="{ backgroundColor: ink, borderColor: ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
          @click="saveRecord"
        >
          <IconCheck class="h-4 w-4" />
          {{ saving ? 'Saving…' : 'Save record' }}
        </button>
      </div>
      <div class="col-span-12 md:col-span-4">
        <button
          data-wf="b-4-1"
          type="button"
          class="inline-flex min-h-[44px] w-full items-center justify-center gap-2 border-2 bg-white px-4 text-[13px] font-bold uppercase tracking-[0.12em] hover:bg-neutral-100 focus:outline-none focus:ring-2 focus:ring-offset-2"
          :style="{ color: ink, borderColor: ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
          @click="cancelEdits"
        >
          Cancel
        </button>
      </div>
      <div class="col-span-12 md:col-span-4">
        <button
          data-wf="b-4-2"
          type="button"
          class="inline-flex min-h-[44px] w-full items-center justify-center gap-2 border-2 bg-white px-4 text-[13px] font-bold uppercase tracking-[0.12em] hover:bg-orange-50 focus:outline-none focus:ring-2 focus:ring-offset-2"
          :style="{ color: accent, borderColor: accent, borderRadius: brand.radius, '--tw-ring-color': accent }"
          @click="confirmOpen = true"
        >
          <IconTrash class="h-4 w-4" />
          Delete record
        </button>
      </div>
    </div>

    <!-- Row 6 -->
    <div
      v-if="confirmOpen"
      data-wf="b-5-0"
      class="fixed inset-0 z-50 flex items-center justify-center bg-black/40 px-6"
      role="dialog"
      aria-modal="true"
      aria-labelledby="delete-heading"
    >
      <div
        class="w-full max-w-md border-2 bg-white shadow-xl"
        :style="{ borderColor: ink }"
      >
        <div class="border-b-2 px-4 py-3" :style="{ borderColor: ink }">
          <h3 id="delete-heading" class="text-[13px] font-bold uppercase tracking-[0.16em]">
            Confirm deletion
          </h3>
        </div>
        <div class="px-4 py-4">
          <p class="text-[14px] leading-relaxed">
            Delete this record? It will no longer appear in the public catalogue.
          </p>
          <p class="mt-2 text-[13px]" :style="{ color: brand.neutralColor }">
            {{ original.title }} &middot; {{ original.author }} &middot; {{ original.shelf_location }}
          </p>
          <div class="mt-4 flex flex-col gap-2 sm:flex-row">
            <button
              data-wf="i-5-0-0"
              type="button"
              class="inline-flex min-h-[44px] flex-1 items-center justify-center gap-2 border-2 px-4 text-[13px] font-bold uppercase tracking-[0.12em] text-white hover:opacity-90 focus:outline-none focus:ring-2 focus:ring-offset-2"
              :style="{ backgroundColor: accent, borderColor: accent, borderRadius: brand.radius, '--tw-ring-color': accent }"
              @click="confirmDelete"
            >
              <IconTrash class="h-4 w-4" />
              Delete record
            </button>
            <button
              data-wf="i-5-0-1"
              type="button"
              class="inline-flex min-h-[44px] flex-1 items-center justify-center border-2 bg-white px-4 text-[13px] font-bold uppercase tracking-[0.12em] hover:bg-neutral-100 focus:outline-none focus:ring-2 focus:ring-offset-2"
              :style="{ color: ink, borderColor: ink, borderRadius: brand.radius, '--tw-ring-color': ink }"
              @click="confirmOpen = false"
            >
              Cancel
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
