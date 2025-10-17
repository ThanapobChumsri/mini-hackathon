<template>
  <div class="min-h-screen bg-[#eef2f9]">
    <div
      class="mx-auto flex w-full max-w-6xl flex-col gap-6 py-10 px-4 md:px-10"
    >
      <div class="flex items-center gap-3">
        <button
          type="button"
          class="inline-flex h-10 w-10 items-center justify-center rounded-full border border-[#dbe3f6] bg-white text-[#4b5c7c] shadow-[0_6px_16px_rgba(15,23,42,0.08)] transition hover:bg-[#eff3ff]"
          @click="handleBack"
        >
          <UIcon name="i-heroicons-arrow-left-20-solid" class="h-4 w-4" />
        </button>
        <span class="text-base font-semibold text-[#1f2a44]">
          {{ headerTitle }}
        </span>
      </div>

      <Transition name="fade" mode="out-in">
        <section
          v-if="activeView === 'form'"
          key="form"
          class="rounded-[28px] border border-[#e0e6f4] bg-white p-6 shadow-[0_24px_60px_rgba(15,23,42,0.05)] md:p-8"
        >
          <header class="mb-8 flex items-center justify-between">
            <h1 class="text-2xl font-semibold text-[#1f2a44]">Create Job</h1>
            <span class="text-xs font-medium uppercase text-[#9aa4bf]"
              >Step 1 of 2</span
            >
          </header>

          <div class="space-y-10">
            <div class="space-y-4">
              <h2
                class="text-sm font-semibold uppercase tracking-wide text-[#7a87a6]"
              >
                Information
              </h2>
              <div class="rounded-3xl border border-[#e6ebf8] bg-[#f8f9ff] p-6">
                <label
                  class="flex flex-col gap-2 text-sm font-medium text-[#4b5c7c]"
                >
                  <div>Job Name<span class="text-red-500">*</span></div>
                  <UInput
                    v-model="form.position"
                    placeholder="Senior Back-End Developer"
                    size="lg"
                    class="rounded-2xl"
                  />
                </label>
              </div>
            </div>

            <div class="space-y-6">
              <div class="flex items-center justify-between">
                <h2
                  class="text-sm font-semibold uppercase tracking-wide text-[#7a87a6]"
                >
                  Criteria
                </h2>
              </div>

              <div class="space-y-6">
                <div
                  v-for="item in criteria"
                  :key="item.id"
                  class="rounded-3xl border border-[#e6ebf8] bg-[#f8f9ff] p-6"
                >
                  <div
                    class="flex flex-col gap-4 md:flex-row md:items-start md:justify-between"
                  >
                    <label class="flex-1 text-sm font-medium text-[#4b5c7c]">
                      {{ item.title }}<span class="text-red-500">*</span>
                      <UInput
                        v-model="item.value"
                        :placeholder="item.placeholder"
                        size="lg"
                        class="mt-3 rounded-2xl"
                      />
                    </label>
                    <div class="flex items-center gap-3">
                      <span class="text-sm font-semibold text-[#7a87a6]"
                        >Mandatory</span
                      >
                      <UToggle v-model="item.mandatory" color="primary" />
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <p v-if="errorMessage" class="mt-6 text-sm text-red-500">
            {{ errorMessage }}
          </p>

          <div class="mt-10 flex justify-end gap-3">
            <UButton
              size="md"
              color="white"
              class="rounded-full border border-[#dfe4f3] px-6 text-[#4b5c7c]"
              @click="handleCancel"
            >
              Cancel
            </UButton>
            <UButton
              size="md"
              color="primary"
              class="rounded-full bg-[#4b7ee8] px-6 text-white"
              @click="handleCreate"
            >
              Create Job
            </UButton>
          </div>
        </section>

        <section
          v-else-if="activeView === 'upload'"
          key="upload"
          class="rounded-[28px] border border-[#e0e6f4] bg-white p-6 shadow-[0_24px_60px_rgba(15,23,42,0.05)] md:p-10 space-y-8"
        >
          <header>
            <h1 class="text-2xl font-semibold text-[#1f2a44]">Upload Resume</h1>
          </header>

          <div
            class="rounded-3xl border border-dashed border-[#d0daf5] bg-[#f7f9ff] px-6 py-12"
          >
            <div
              class="flex flex-col items-center justify-center gap-4 text-center"
            >
              <Dnd :key="uploaderKey" ui-type="2" @upload="handleUpload" />
              <p class="text-xs text-[#9aa4bf]">
                Support for a single or bulk upload. Strictly prohibit from
                uploading company data or other banned files.
              </p>
            </div>
          </div>

          <div>
            <div class="mb-3">
              <div
                class="flex items-center justify-between text-sm font-semibold text-[#4b5c7c]"
              >
                <span>Uploaded list</span>
                <span class="text-xs font-normal text-[#7a87a6]">
                  {{ uploadedFiles.length }} file{{
                    uploadedFiles.length !== 1 ? "s" : ""
                  }}
                </span>
              </div>
            </div>

            <div
              v-if="uploadedFiles.length"
              class="overflow-hidden rounded-3xl border border-[#e6ebf8]"
            >
              <table class="min-w-full divide-y divide-[#e6ebf8]">
                <thead
                  class="bg-[#f8f9ff] text-left text-xs font-semibold uppercase tracking-wider text-[#7a87a6]"
                >
                  <tr>
                    <th class="px-6 py-4">Action</th>
                    <th class="px-4 py-4">File Name</th>
                    <th class="px-4 py-4">Uploaded on</th>
                  </tr>
                </thead>
                <tbody
                  class="divide-y divide-[#eef1fb] bg-white text-sm text-[#2f3c5a]"
                >
                  <tr
                    v-for="file in uploadedFiles"
                    :key="file.id"
                    class="transition hover:bg-[#f4f6ff]"
                  >
                    <td class="px-6 py-4">
                      <button
                        type="button"
                        class="flex h-9 w-9 items-center justify-center rounded-xl bg-[#f87171] text-white shadow-[0_10px_20px_rgba(248,113,113,0.25)]"
                        @click="removeUploadedFile(file.id)"
                      >
                        <UIcon
                          name="i-heroicons-trash-16-solid"
                          class="h-4 w-4"
                        />
                      </button>
                    </td>
                    <td class="px-4 py-4 font-medium truncate max-w-[240px]">
                      {{ file.name }}
                    </td>
                    <td class="px-4 py-4 text-[#66759a]">
                      {{ file.uploadedOn }}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div
              v-else
              class="rounded-3xl border border-dashed border-[#dbe2f3] bg-white py-10 text-center text-sm text-[#9aa4bf]"
            >
              No files uploaded yet.
            </div>
          </div>

          <div class="flex justify-end gap-3 pt-4">
            <UButton
              size="md"
              color="white"
              class="rounded-full border border-[#dfe4f3] px-6 text-[#4b5c7c]"
              @click="handleBack"
            >
              Back
            </UButton>
            <UButton
              size="md"
              color="primary"
              class="rounded-full bg-[#4b7ee8] px-6 text-white"
              @click="goToApplicants"
            >
              Proceed
            </UButton>
          </div>
        </section>

        <section
          v-else
          key="applicants"
          class="rounded-[28px] border border-[#e0e6f4] bg-white p-6 shadow-[0_24px_60px_rgba(15,23,42,0.05)] md:p-8"
        >
          <header
            class="mb-6 flex flex-col gap-4 md:flex-row md:items-center md:justify-between"
          >
            <div>
              <h1 class="text-xl font-semibold text-[#1f2a44]">
                {{ form.position }} Applicants
              </h1>
              <p class="text-sm text-[#7a87a6]">
                Compare candidate resumes against your selected criteria.
              </p>
            </div>
            <div class="flex items-center gap-2">
              <button
                type="button"
                class="inline-flex items-center gap-2 rounded-full border border-[#dbe3f6] bg-white px-4 py-2 text-xs font-semibold text-[#4b5c7c] transition hover:bg-[#f3f6ff]"
              >
                <UIcon name="i-heroicons-funnel-20-solid" class="h-4 w-4" />
                Filter
              </button>
              <button
                type="button"
                class="inline-flex items-center gap-2 rounded-full border border-[#dbe3f6] bg-white px-4 py-2 text-xs font-semibold text-[#4b5c7c] transition hover:bg-[#f3f6ff]"
              >
                <UIcon
                  name="i-heroicons-bars-arrow-down-20-solid"
                  class="h-4 w-4"
                />
                Sort
              </button>
              <button
                type="button"
                class="inline-flex items-center gap-2 rounded-full bg-[#4b7ee8] px-4 py-2 text-xs font-semibold text-white shadow-[0_12px_30px_rgba(75,126,232,0.35)] transition hover:bg-[#3f6fd4]"
              >
                <UIcon name="i-heroicons-chart-bar-20-solid" class="h-4 w-4" />
                Analyze
              </button>
            </div>
          </header>

          <div
            class="mb-6 flex flex-col gap-3 md:flex-row md:items-center md:justify-between"
          >
            <div class="flex items-center gap-3">
              <div class="relative">
                <UIcon
                  name="i-heroicons-magnifying-glass-20-solid"
                  class="pointer-events-none absolute left-4 top-1/2 h-4 w-4 -translate-y-1/2 text-[#9aa4bf]"
                />
                <input
                  v-model="search"
                  type="search"
                  placeholder="Search here"
                  class="w-64 rounded-full border border-[#dae2f5] bg-[#f8f9ff] py-2 pl-10 pr-4 text-sm text-[#2f3c5a] outline-none transition focus:border-[#4b7ee8]"
                />
              </div>
              <button
                @click="activeView = 'upload'"
                type="button"
                class="inline-flex items-center gap-2 rounded-full border border-[#dbe3f6] bg-white px-4 py-2 text-xs font-semibold text-[#4b5c7c] transition hover:bg-[#f3f6ff]"
              >
                <UIcon
                  name="i-heroicons-document-arrow-up-20-solid"
                  class="h-4 w-4"
                />
                Upload Resume
              </button>
            </div>
          </div>

          <div class="overflow-hidden rounded-3xl border border-[#e6ebf8]">
            <table class="min-w-full divide-y divide-[#e6ebf8]">
              <thead
                class="bg-[#f8f9ff] text-left text-xs font-semibold uppercase tracking-wider text-[#7a87a6]"
              >
                <tr>
                  <th class="px-6 py-4">
                    <input
                      type="checkbox"
                      class="h-4 w-4 rounded border-[#ccd5ea] text-[#4b7ee8] focus:ring-[#4b7ee8]"
                    />
                  </th>
                  <th class="px-4 py-4">Name</th>
                  <th class="px-4 py-4">Current Role</th>
                  <th class="px-4 py-4">Mandatory</th>
                  <th class="px-4 py-4">Preferred</th>
                  <th class="px-4 py-4 text-right">Score</th>
                  <th class="px-4 py-4 text-right">Status</th>
                </tr>
              </thead>
              <tbody
                class="divide-y divide-[#eef1fb] bg-white text-sm text-[#2f3c5a]"
              >
                <tr
                  v-for="applicant in filteredApplicants"
                  :key="applicant.id"
                  class="transition hover:bg-[#f4f6ff]"
                >
                  <td class="px-6 py-4">
                    <input
                      type="checkbox"
                      class="h-4 w-4 rounded border-[#ccd5ea] text-[#4b7ee8] focus:ring-[#4b7ee8]"
                    />
                  </td>
                  <td class="px-4 py-4">
                    <div class="flex items-center gap-3">
                      <button
                        type="button"
                        class="flex h-8 w-8 items-center justify-center rounded-xl bg-[#4b7ee8] text-white shadow-[0_10px_20px_rgba(75,126,232,0.25)]"
                      >
                        <UIcon
                          name="i-heroicons-user-group-16-solid"
                          class="h-4 w-4"
                        />
                      </button>
                      <div>
                        <p class="font-semibold text-[#1f2a44]">
                          {{ applicant.name }}
                        </p>
                        <p class="text-xs text-[#7a87a6]">
                          {{ applicant.company }}
                        </p>
                      </div>
                    </div>
                  </td>
                  <td class="px-4 py-4 text-[#66759a]">{{ applicant.role }}</td>
                  <td class="px-4 py-4">
                    <div class="flex flex-wrap items-center gap-1">
                      <span
                        v-for="item in applicant.mandatory"
                        :key="item.id"
                        class="h-5 w-3 rounded-full"
                        :class="statusPalette[item.status]"
                        :title="item.note"
                      ></span>
                    </div>
                  </td>
                  <td class="px-4 py-4">
                    <div class="flex flex-wrap items-center gap-1">
                      <span
                        v-for="item in applicant.preferred"
                        :key="item.id"
                        class="h-5 w-3 rounded-full"
                        :class="statusPalette[item.status]"
                        :title="item.note"
                      ></span>
                    </div>
                  </td>
                  <td
                    class="px-4 py-4 text-right text-lg font-semibold text-[#1f2a44]"
                  >
                    {{ applicant.score }}
                  </td>
                  <td class="px-4 py-4 text-right">
                    <span
                      class="inline-flex items-center gap-2 rounded-full px-4 py-1 text-xs font-semibold"
                      :class="statusBadge[applicant.state]"
                    >
                      <span class="h-2 w-2 rounded-full bg-white/80"></span>
                      {{ applicant.stateLabel }}
                    </span>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </section>
      </Transition>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref, computed } from "vue";
import { useRouter } from "vue-router";
import Dnd from "~/components/dnd.vue";

const router = useRouter();
const errorMessage = ref("");
const activeView = ref<"form" | "upload" | "applicants">("form");
const search = ref("");

const form = reactive({
  position: "Senior Back-End Developer",
});

const criteria = reactive([
  {
    id: "workingExperience",
    title: "Working experience",
    placeholder: "5 years of experience",
    mandatory: true,
    value: 5,
  },
  {
    id: "skills",
    title: "Skills",
    placeholder: "Javascript",
    mandatory: true,
    value: "Javascript",
  },
  {
    id: "education",
    title: "Education",
    placeholder: "Bachelor's Degree in Computer Science Software Engineers",
    mandatory: false,
    value: "Bachelor's Degree in Computer Science Software Engineers",
  },
  {
    id: "additional",
    title: "Additional",
    placeholder: "Based in Bangkok",
    mandatory: true,
    value: "Based in Bangkok",
  },
]) as Array<{
  id: string;
  title: string;
  placeholder: string;
  mandatory: boolean;
  value: string | number;
}>;

const uploadedFiles = ref<
  { id: number; name: string; uploadedOn: string; file: File }[]
>([]);
const uploaderKey = ref(0);

interface Applicant {
  id: number;
  name: string;
  company: string;
  role: string;
  score: number;
  state: string;
  stateLabel: string;
  mandatory: { id: string; status: string; note: string }[];
  preferred: { id: string; status: string; note: string }[];
}

const formatDateTime = (date: Date) =>
  new Intl.DateTimeFormat("en-GB", {
    day: "2-digit",
    month: "2-digit",
    year: "numeric",
    hour: "2-digit",
    minute: "2-digit",
    hour12: true,
  })
    .format(date)
    .replace(",", "");

const handleUpload = (payload: { file?: File | null }) => {
  if (payload?.file) {
    const entryId = Date.now();
    uploadedFiles.value = [
      {
        id: entryId,
        name: payload.file.name,
        uploadedOn: formatDateTime(new Date()),
        file: payload.file,
      },
      ...uploadedFiles.value,
    ];
  } else {
    uploadedFiles.value = [];
  }

  uploaderKey.value += 1;
};

const removeUploadedFile = (id: number) => {
  uploadedFiles.value = uploadedFiles.value.filter((file) => file.id !== id);
  if (!uploadedFiles.value.length) {
    uploaderKey.value += 1;
  }
};

const applicants = ref([
  {
    id: 1,
    name: "Olivia Brown",
    company: "Amazon",
    role: "Senior",
    score: 95,
    state: "advance",
    stateLabel: "Advance",
    mandatory: [
      { id: "exp", status: "pass", note: "Working Experience is required" },
      { id: "lead", status: "pass", note: "Led cross-functional teams" },
      { id: "ts", status: "pass", note: "Strong TypeScript experience" },
      { id: "cloud", status: "pass", note: "AWS expertise proven" },
    ],
    preferred: [
      {
        id: "design",
        status: "warning",
        note: "Limited design system exposure",
      },
      {
        id: "tailwind",
        status: "pass",
        note: "Tailwind used in recent project",
      },
      { id: "nuxt", status: "pass", note: "Nuxt 3 production experience" },
    ],
  },
  {
    id: 2,
    name: "Wade Warren",
    company: "Meta",
    role: "Senior",
    score: 93,
    state: "advance",
    stateLabel: "Advance",
    mandatory: [
      { id: "exp", status: "pass", note: "10 years in backend roles" },
      { id: "lead", status: "pass", note: "Led 12 person squad" },
      { id: "ts", status: "warning", note: "TypeScript adoption underway" },
      { id: "cloud", status: "pass", note: "AWS certified" },
    ],
    preferred: [
      {
        id: "design",
        status: "pass",
        note: "Collaborated with design systems",
      },
      { id: "tailwind", status: "warning", note: "CSS utilities experience" },
      { id: "nuxt", status: "pass", note: "Nuxt SSR experience" },
    ],
  },
  {
    id: 3,
    name: "Ava Rodriguez",
    company: "Adobe",
    role: "Lead",
    score: 91,
    state: "advance",
    stateLabel: "Advance",
    mandatory: [
      { id: "exp", status: "pass", note: "8 years leadership" },
      { id: "lead", status: "pass", note: "Director level" },
      { id: "ts", status: "pass", note: "Architected TS migration" },
      { id: "cloud", status: "warning", note: "Cloud exposure limited to GCP" },
    ],
    preferred: [
      { id: "design", status: "pass", note: "Design system owner" },
      { id: "tailwind", status: "pass", note: "Tailwind in design ops" },
      { id: "nuxt", status: "warning", note: "Vue SSR but not Nuxt" },
    ],
  },
  {
    id: 4,
    name: "James Taylor",
    company: "Netflix",
    role: "Senior",
    score: 90,
    state: "advance",
    stateLabel: "Advance",
    mandatory: [
      { id: "exp", status: "pass", note: "9 years streaming platform" },
      { id: "lead", status: "warning", note: "Tech lead for 4 engineers" },
      { id: "ts", status: "pass", note: "Advanced TS generics usage" },
      { id: "cloud", status: "pass", note: "AWS and Kubernetes" },
    ],
    preferred: [
      { id: "design", status: "warning", note: "Design collaboration limited" },
      { id: "tailwind", status: "pass", note: "Tailwind for admin tools" },
      { id: "nuxt", status: "warning", note: "Vue 3 focus" },
    ],
  },
  {
    id: 5,
    name: "Mason Wilson",
    company: "Spotify",
    role: "Senior",
    score: 88,
    state: "review",
    stateLabel: "To Review",
    mandatory: [
      { id: "exp", status: "pass", note: "8 years audio platforms" },
      { id: "lead", status: "warning", note: "Mentored but not managed" },
      { id: "ts", status: "pass", note: "TS on microservices" },
      { id: "cloud", status: "warning", note: "Focused on GCP" },
    ],
    preferred: [
      { id: "design", status: "pass", note: "Design system contributor" },
      { id: "tailwind", status: "warning", note: "Adopted WindiCSS" },
      { id: "nuxt", status: "warning", note: "Vue SSR experience" },
    ],
  },
  {
    id: 6,
    name: "Emma Johnson",
    company: "Intel",
    role: "Middle",
    score: 80,
    state: "review",
    stateLabel: "To Review",
    mandatory: [
      { id: "exp", status: "pass", note: "6 years embedded" },
      { id: "lead", status: "warning", note: "Scrum master tasks" },
      { id: "ts", status: "warning", note: "TS for tooling only" },
      { id: "cloud", status: "warning", note: "Minimal cloud" },
    ],
    preferred: [
      { id: "design", status: "warning", note: "Basic design collaboration" },
      { id: "tailwind", status: "fail", note: "No Tailwind" },
      { id: "nuxt", status: "fail", note: "No Nuxt" },
    ],
  },
  {
    id: 7,
    name: "Noah Martinez",
    company: "Slack",
    role: "Middle",
    score: 73,
    state: "review",
    stateLabel: "To Review",
    mandatory: [
      { id: "exp", status: "warning", note: "4 years experience" },
      { id: "lead", status: "warning", note: "Project lead part-time" },
      { id: "ts", status: "warning", note: "Intermediate TS" },
      { id: "cloud", status: "warning", note: "Cloud bootcamp completed" },
    ],
    preferred: [
      { id: "design", status: "warning", note: "Figma handoff only" },
      { id: "tailwind", status: "pass", note: "Tailwind in startup" },
      { id: "nuxt", status: "fail", note: "No Nuxt" },
    ],
  },
  {
    id: 8,
    name: "Isabella Lee",
    company: "Shopify",
    role: "Junior",
    score: 62,
    state: "review",
    stateLabel: "To Review",
    mandatory: [
      { id: "exp", status: "fail", note: "2 years experience" },
      { id: "lead", status: "fail", note: "No leadership responsibilities" },
      { id: "ts", status: "warning", note: "Learning TypeScript" },
      { id: "cloud", status: "warning", note: "Basic AWS" },
    ],
    preferred: [
      { id: "design", status: "warning", note: "Collaborated with designers" },
      { id: "tailwind", status: "warning", note: "Tailwind in side projects" },
      { id: "nuxt", status: "fail", note: "No Nuxt usage" },
    ],
  },
  {
    id: 9,
    name: "Liam Smith",
    company: "Stripe",
    role: "Junior",
    score: 58,
    state: "archived",
    stateLabel: "Archived",
    mandatory: [
      { id: "exp", status: "fail", note: "3 years total experience" },
      { id: "lead", status: "fail", note: "No leadership responsibilities" },
      { id: "ts", status: "warning", note: "TypeScript bootcamp" },
      { id: "cloud", status: "fail", note: "No cloud experience" },
    ],
    preferred: [
      { id: "design", status: "fail", note: "No design collaboration" },
      { id: "tailwind", status: "warning", note: "Tailwind for prototypes" },
      { id: "nuxt", status: "fail", note: "No Nuxt" },
    ],
  },
  {
    id: 10,
    name: "Sophia Garcia",
    company: "Zendesk",
    role: "Junior",
    score: 50,
    state: "archived",
    stateLabel: "Archived",
    mandatory: [
      { id: "exp", status: "fail", note: "2 years experience" },
      { id: "lead", status: "fail", note: "IC only" },
      { id: "ts", status: "fail", note: "No TypeScript" },
      { id: "cloud", status: "fail", note: "No cloud" },
    ],
    preferred: [
      { id: "design", status: "fail", note: "No design collaboration" },
      { id: "tailwind", status: "fail", note: "No Tailwind" },
      { id: "nuxt", status: "fail", note: "No Nuxt" },
    ],
  },
]);

const statusPalette: Record<string, string> = {
  pass: "bg-[#7dd37d]",
  warning: "bg-[#f7b648]",
  fail: "bg-[#f26b6b]",
};

const statusBadge: Record<string, string> = {
  advance: "bg-[#7dd37d] text-[#135c13]",
  review: "bg-[#fde68a] text-[#785a05]",
  archived: "bg-[#f26b6b] text-white",
};

const headerTitle = computed(() => {
  if (activeView.value === "upload") {
    return "Upload Resumes";
  }
  if (activeView.value === "applicants") {
    return `${form.position} Applicants`;
  }
  return "Create Job";
});

const goBack = () => {
  router.push("/");
};

const handleBack = () => {
  if (activeView.value === "applicants") {
    activeView.value = "upload";
  } else if (activeView.value === "upload") {
    activeView.value = "form";
  } else {
    goBack();
  }
};

const handleCancel = () => {
  goBack();
};

const handleCreate = () => {
  if (!form.position.trim()) {
    errorMessage.value = "Please provide a job name before continuing.";
    return;
  }

  errorMessage.value = "";
  activeView.value = "upload";
};

const goToApplicants = async () => {
  if (!uploadedFiles.value.length) {
    alert("Please upload at least one resume before proceeding.");
    return;
  }

  const criteriaPayload = criteria.map(({ id, title, value, mandatory }) => ({
    id,
    title,
    value,
    mandatory,
  }));

  const formData = new FormData();
  uploadedFiles.value.forEach((item: { id: number; name: string; uploadedOn: string; file: File }) =>
    formData.append("files", item.file)
  );
  console.log(criteriaPayload);

  const skillsValue = typeof criteria[1].value === 'string' ? criteria[1].value : String(criteria[1].value);
  const additionalValue = typeof criteria[3].value === 'string' ? criteria[3].value : String(criteria[3].value);

  formData.append(
    "criteria",
    JSON.stringify({
      title: form.position,
      required_skills: skillsValue.split(","),
      nice_to_have_skills: additionalValue.split(","),
      min_years_experience: criteria[0].value,
      required_degree: criteria[2].value,
      location: "Bangkok",
    })
  );
    // activeView.value = "applicants";
  try {
    const response: any = await $fetch(
      "http://192.168.31.158:8000/task/upload_resumes",
      {
        method: "POST",
        body: formData,
      }
    );

    console.log("API Response:", response);

    // Transform API response to applicants format
    if (response.results && Array.isArray(response.results)) {
      applicants.value = response.results.map((result: any, index: number) => {
        const llm = result.llm_response;

        // Calculate mandatory criteria status
        const mandatory = [
          {
            id: "exp",
            status: llm.years_of_experience && llm.years_of_experience >= criteria[0].value ? "pass" : "warning",
            note: `${llm.years_of_experience || 0} years of experience`,
          },
          {
            id: "skills",
            status: "pass", // You can add logic to check skills match
            note: llm.skills?.join(", ") || "No skills listed",
          },
          {
            id: "degree",
            status: llm.education && llm.education.length > 0 ? "pass" : "warning",
            note: llm.education?.[0]?.degree || "No degree information",
          },
          {
            id: "location",
            status: llm.location ? "pass" : "warning",
            note: llm.location || "Location not specified",
          },
        ];

        // Calculate preferred criteria status
        const preferred = [
          {
            id: "company",
            status: llm.current_company ? "pass" : "warning",
            note: llm.current_company || "No current company",
          },
        ];

        // Calculate score (you can customize this logic)
        const passCount = mandatory.filter((m) => m.status === "pass").length;
        const score = Math.round((passCount / mandatory.length) * 100);

        // Determine state based on score
        let state = "archived";
        let stateLabel = "Archived";
        if (score >= 80) {
          state = "advance";
          stateLabel = "Advance";
        } else if (score >= 60) {
          state = "review";
          stateLabel = "To Review";
        }

        return {
          id: index + 1,
          name: llm.full_name || "Unknown",
          company: llm.current_company || "N/A",
          role: llm.current_title || "N/A",
          score,
          state,
          stateLabel,
          mandatory,
          preferred,
        };
      });
    }

  } catch (error) {

    console.error("Error uploading resumes:", error);
    alert("Failed to upload resumes. Please try again.");
  }
};

const filteredApplicants = computed(() => {
  if (!search.value.trim()) {
    return applicants.value;
  }
  const keyword = search.value.trim().toLowerCase();
  return applicants.value.filter((candidate: Applicant) =>
    candidate.name.toLowerCase().includes(keyword)
  );
});
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.25s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
