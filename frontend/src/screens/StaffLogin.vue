<script setup>
/* eslint-disable @typescript-eslint/no-unused-vars -- generated imports, matched generously */
import { ref, onUnmounted } from "vue";
import { Button, Input, Label } from "@/lib/ui";
import { Icons } from "@/lib/icons";
import { brand } from "@/lib/brand";
import { useNavigate } from "@/lib/navigate";

const { IconClock, IconArrowLeft, IconAlertCircle } = Icons;
const navigate = useNavigate();

const VALID_EMAIL = 'm.okonkwo@ashgrovelibrary.org'
const VALID_PASSWORD = 'catalogue2026'

const email = ref('')
const password = ref('')
const emailError = ref('')
const passwordError = ref('')
const authError = ref(false)
const loading = ref(false)
const signedOutNotice = ref(true)

let timer = null

function submit() {
  if (loading.value) return
  authError.value = false
  signedOutNotice.value = false
  emailError.value = email.value.trim() ? '' : 'Enter the email address on your staff account.'
  passwordError.value = password.value ? '' : 'Enter your password.'
  if (emailError.value || passwordError.value) return

  loading.value = true
  timer = setTimeout(() => {
    loading.value = false
    timer = null
    if (email.value.trim().toLowerCase() === VALID_EMAIL && password.value === VALID_PASSWORD) {
      navigate('manage')
    } else {
      authError.value = true
      password.value = ''
    }
  }, 600)
}

onUnmounted(() => {
  if (timer) clearTimeout(timer)
})
</script>

<template>
  <div class="min-h-screen w-full" :style="{ backgroundColor: brand.backgroundColor, fontFamily: brand.fontBody }">
    <div class="mx-auto w-full max-w-5xl px-6 py-12 sm:px-10 lg:px-12">

      <!-- masthead rule: catalogue-card spine -->
      <div class="border-b-2 pb-3 mb-10 flex items-baseline justify-between gap-6"
           :style="{ borderColor: brand.primaryColor }">
        <span class="text-[11px] uppercase tracking-[0.22em]" :style="{ color: brand.primaryColor }">
          Ashgrove Public Library — Catalogue System
        </span>
        <span class="hidden sm:inline text-[11px] uppercase tracking-[0.22em] tabular-nums"
              :style="{ color: brand.neutralColor }">
          Staff access · REC 0001
        </span>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-12 gap-10">
        <div class="lg:col-span-7">

          <!-- Row 1 -->
          <h1 data-wf="b-0-0"
              class="text-[34px] leading-[1.15] font-bold tracking-tight sm:text-[38px]"
              :style="{ color: brand.primaryColor, fontFamily: brand.fontHeading }">
            Staff sign in
          </h1>

          <!-- Row 2 -->
          <p data-wf="b-1-0"
             class="mt-4 max-w-[62ch] text-[15px] leading-[1.6]"
             :style="{ color: brand.neutralColor }">
            The management area is for library staff only. Sign in to maintain catalogue records.
          </p>

          <!-- Row 3 -->
          <form data-wf="b-2-0" class="mt-8 border-2 p-6" :style="{ borderColor: brand.primaryColor }"
                @submit.prevent="submit">
            <h2 class="text-[12px] font-bold uppercase tracking-[0.22em]"
                :style="{ color: brand.primaryColor, fontFamily: brand.fontHeading }">
              Sign in
            </h2>
            <div class="mt-5 h-[2px] w-full" :style="{ backgroundColor: brand.primaryColor }"></div>

            <div class="mt-6 space-y-6">
              <div>
                <Label for="staff-email"
                       class="block text-[11px] font-bold uppercase tracking-[0.18em]"
                       :style="{ color: brand.primaryColor }">
                  Email
                </Label>
                <Input
                  id="staff-email"
                  data-wf="i-2-0-0"
                  v-model="email"
                  type="email"
                  autocomplete="username"
                  placeholder="m.okonkwo@ashgrovelibrary.org"
                  :aria-invalid="emailError ? 'true' : 'false'"
                  class="mt-2 w-full min-h-[44px] rounded-none border-2 bg-white px-3 text-[15px] outline-none focus:ring-2 focus:ring-offset-2"
                  :style="{ borderColor: emailError ? brand.accentColor : brand.primaryColor, color: brand.primaryColor }"
                />
                <p v-if="emailError" class="mt-2 text-[12px] font-medium" :style="{ color: brand.accentColor }">
                  {{ emailError }}
                </p>
              </div>

              <div>
                <Label for="staff-password"
                       class="block text-[11px] font-bold uppercase tracking-[0.18em]"
                       :style="{ color: brand.primaryColor }">
                  Password
                </Label>
                <Input
                  id="staff-password"
                  data-wf="i-2-0-1"
                  v-model="password"
                  type="password"
                  autocomplete="current-password"
                  placeholder="Your staff password"
                  :aria-invalid="passwordError ? 'true' : 'false'"
                  class="mt-2 w-full min-h-[44px] rounded-none border-2 bg-white px-3 text-[15px] outline-none focus:ring-2 focus:ring-offset-2"
                  :style="{ borderColor: passwordError ? brand.accentColor : brand.primaryColor, color: brand.primaryColor }"
                />
                <p v-if="passwordError" class="mt-2 text-[12px] font-medium" :style="{ color: brand.accentColor }">
                  {{ passwordError }}
                </p>
              </div>
            </div>

            <!-- Row 4 -->
            <div data-wf="b-3-0" class="mt-6" aria-live="polite">
              <div v-if="authError"
                   class="flex items-start gap-2 border-2 px-3 py-2"
                   :style="{ borderColor: brand.accentColor }">
                <IconAlertCircle class="h-4 w-4 mt-[2px] shrink-0" :style="{ color: brand.accentColor }" />
                <span class="text-[13px] font-medium leading-[1.5]" :style="{ color: brand.accentColor }">
                  Email or password is incorrect.
                </span>
              </div>
              <p v-else-if="signedOutNotice" class="text-[13px] leading-[1.5]" :style="{ color: brand.neutralColor }">
                Demo account — m.okonkwo@ashgrovelibrary.org / catalogue2026
              </p>
            </div>

            <!-- Row 5 -->
            <div class="mt-6 grid grid-cols-1 gap-4 sm:grid-cols-2 sm:items-center">
              <Button
                data-wf="b-4-0"
                type="submit"
                :disabled="loading"
                class="w-full min-h-[44px] rounded-none border-2 px-4 text-[13px] font-bold uppercase tracking-[0.18em] text-white outline-none transition-opacity hover:opacity-85 focus:ring-2 focus:ring-offset-2 disabled:opacity-60"
                :style="{ backgroundColor: brand.primaryColor, borderColor: brand.primaryColor }"
              >
                <span v-if="loading" class="inline-flex items-center gap-2">
                  <IconClock class="h-4 w-4" />
                  Signing in…
                </span>
                <span v-else>Sign in</span>
              </Button>

              <button
                data-wf="b-4-1"
                type="button"
                class="min-h-[44px] justify-self-start text-left text-[13px] underline underline-offset-4 outline-none hover:opacity-70 focus:ring-2 focus:ring-offset-2 sm:justify-self-end"
                :style="{ color: brand.primaryColor }"
              >
                Forgot your password?
              </button>
            </div>
          </form>

          <!-- Row 6 -->
          <div class="mt-8">
            <button
              data-wf="b-5-0"
              type="button"
              class="inline-flex min-h-[44px] items-center gap-2 text-[13px] font-medium uppercase tracking-[0.14em] outline-none hover:opacity-70 focus:ring-2 focus:ring-offset-2"
              :style="{ color: brand.primaryColor }"
              @click="navigate('catalogue')"
            >
              <IconArrowLeft class="h-4 w-4" />
              Back to public catalogue
            </button>
          </div>
        </div>

        <!-- catalogue card motif -->
        <aside class="hidden lg:block lg:col-span-5">
          <div class="border-2 p-6" :style="{ borderColor: brand.primaryColor }">
            <p class="text-[11px] uppercase tracking-[0.22em]" :style="{ color: brand.neutralColor }">
              Access notice
            </p>
            <div class="mt-4 h-[2px] w-full" :style="{ backgroundColor: brand.primaryColor }"></div>
            <dl class="mt-4 divide-y-2" :style="{ borderColor: brand.primaryColor }">
              <div class="flex items-baseline justify-between py-3">
                <dt class="text-[12px] uppercase tracking-[0.14em]" :style="{ color: brand.neutralColor }">Public catalogue</dt>
                <dd class="text-[13px] font-bold" :style="{ color: brand.primaryColor }">No sign-in required</dd>
              </div>
              <div class="flex items-baseline justify-between py-3" :style="{ borderColor: brand.primaryColor }">
                <dt class="text-[12px] uppercase tracking-[0.14em]" :style="{ color: brand.neutralColor }">Management area</dt>
                <dd class="text-[13px] font-bold" :style="{ color: brand.primaryColor }">Staff account only</dd>
              </div>
              <div class="flex items-baseline justify-between py-3" :style="{ borderColor: brand.primaryColor }">
                <dt class="text-[12px] uppercase tracking-[0.14em]" :style="{ color: brand.neutralColor }">Records held</dt>
                <dd class="text-[13px] font-bold tabular-nums" :style="{ color: brand.primaryColor }">8,412</dd>
              </div>
              <div class="flex items-baseline justify-between py-3" :style="{ borderColor: brand.primaryColor }">
                <dt class="text-[12px] uppercase tracking-[0.14em]" :style="{ color: brand.neutralColor }">Index updated</dt>
                <dd class="text-[13px] font-bold tabular-nums" :style="{ color: brand.primaryColor }">4 Nov 2026, 08:15</dd>
              </div>
            </dl>
            <p class="mt-4 text-[12px] leading-[1.6]" :style="{ color: brand.neutralColor }">
              Sessions end when you sign out. Management pages then ask for your email and password again.
            </p>
          </div>
        </aside>
      </div>
    </div>
  </div>
</template>
