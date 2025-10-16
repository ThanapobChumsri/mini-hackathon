<template>
  <div class="rounded-[32px] border border-[#dfe4f3] bg-white shadow-[0_24px_60px_rgba(15,23,42,0.08)] overflow-hidden">
    <header class="flex items-center justify-between bg-[#f8f9ff] px-6 py-4">
      <div>
        <h3 class="text-xl font-semibold text-[#42527b]">Resume Evaluation</h3>
        <p class="text-sm text-slate-400">Review how each candidate matches the selected criteria.</p>
      </div>
      <button
        type="button"
        class="flex items-center gap-2 rounded-full border border-[#cbd7f5] bg-white px-5 py-2 text-sm font-semibold text-[#4b7ee8] shadow-[0_8px_16px_rgba(75,126,232,0.15)] transition hover:bg-[#eef3ff]"
      >
        Export report
        <UIcon name="i-heroicons-document-arrow-down-20-solid" class="h-4 w-4" />
      </button>
    </header>

    <div class="overflow-x-auto">
      <table class="min-w-full divide-y divide-[#e7e9f2]">
        <thead class="bg-[#f8f9ff] text-left text-xs font-semibold uppercase tracking-wide text-[#72809f]">
          <tr>
            <th class="px-6 py-4">Candidate</th>
            <th class="px-4 py-4">Current Company</th>
            <th class="px-4 py-4">Current Role</th>
            <th class="px-4 py-4">Mandatory</th>
            <th class="px-4 py-4">Preferred</th>
            <th class="px-4 py-4 text-center">Score</th>
            <th class="px-4 py-4 text-right">Status</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-[#eef1fb] bg-white text-sm">
          <tr
            v-for="candidate in candidates"
            :key="candidate.id"
            class="transition hover:bg-[#f4f6ff]"
          >
            <td class="px-6 py-5">
              <div class="flex items-center gap-4">
                <input
                  type="radio"
                  name="candidate"
                  class="h-4 w-4 accent-[#4b7ee8]"
                  :value="candidate.id"
                />
                <button
                  type="button"
                  class="flex items-center gap-2 rounded-full border border-[#cdd9fb] bg-[#ecf0ff] px-3 py-1 text-xs font-semibold text-[#4b7ee8] transition hover:bg-[#dde5ff]"
                >
                  <UIcon name="i-heroicons-document-text-20-solid" class="h-4 w-4" />
                  PDF
                </button>
                <div>
                  <p class="text-sm font-semibold text-[#2f3c5a]">
                    {{ candidate.name }}
                  </p>
                </div>
              </div>
            </td>
            <td class="px-4 py-5 text-[#4a5d82]">{{ candidate.company }}</td>
            <td class="px-4 py-5 text-[#4a5d82]">{{ candidate.role }}</td>
            <td class="px-4 py-5">
              <div class="flex flex-wrap items-center gap-2">
                <div
                  v-for="item in candidate.mandatory"
                  :key="item.id"
                  class="relative flex h-8 w-10 cursor-pointer items-center justify-center rounded-full"
                  :class="statusColors[item.status]"
                  :title="item.note"
                >
                  <UIcon
                    v-if="item.status === 'fail'"
                    name="i-heroicons-exclamation-triangle-16-solid"
                    class="h-4 w-4 text-white"
                  />
                  <UIcon
                    v-else-if="item.status === 'pass'"
                    name="i-heroicons-check-16-solid"
                    class="h-4 w-4 text-white"
                  />
                </div>
              </div>
            </td>
            <td class="px-4 py-5">
              <div class="flex flex-wrap items-center gap-2">
                <div
                  v-for="item in candidate.preferred"
                  :key="item.id"
                  class="relative flex h-8 w-10 cursor-pointer items-center justify-center rounded-full"
                  :class="statusColors[item.status]"
                  :title="item.note"
                >
                  <UIcon
                    v-if="item.status === 'fail'"
                    name="i-heroicons-x-mark-16-solid"
                    class="h-4 w-4 text-white"
                  />
                  <UIcon
                    v-else-if="item.status === 'pass'"
                    name="i-heroicons-check-16-solid"
                    class="h-4 w-4 text-white"
                  />
                </div>
              </div>
            </td>
            <td class="px-4 py-5 text-center text-lg font-semibold text-[#4b7ee8]">
              {{ candidate.score }}
            </td>
            <td class="px-4 py-5 text-right">
              <span
                class="inline-flex items-center gap-2 rounded-full border border-[#e7e9f2] bg-[#fff4dd] px-4 py-1 text-xs font-semibold text-[#c47c1b]"
              >
                <span class="h-2 w-2 rounded-full bg-[#f0b44d]"></span>
                {{ candidate.status }}
              </span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup lang="ts">
const statusColors: Record<string, string> = {
  pass: "bg-[#83d1c3]",
  warning: "bg-[#ffd28d]",
  fail: "bg-[#f38b90]",
  pending: "bg-[#c8d6f8]",
};

const candidates = [
  {
    id: 1,
    name: "Marcia Samin",
    company: "Amazon",
    role: "Senior",
    score: 90,
    status: "To Review",
    mandatory: [
      { id: "ts", status: "pass", note: "Experience with TypeScript exceeds minimum requirement." },
      { id: "aws", status: "pass", note: "AWS certified since 2022." },
      { id: "lead", status: "warning", note: "Leadership experience is limited to squad size < 4." },
      { id: "sec", status: "pending", note: "Security clearance check pending." },
    ],
    preferred: [
      { id: "tailwind", status: "pass", note: "TailwindCSS mentioned in the portfolio projects." },
      { id: "nuxt", status: "pending", note: "Has Vue experience, Nuxt usage not confirmed." },
      { id: "ci", status: "pass", note: "Maintains CI/CD pipelines using GitHub Actions." },
      { id: "design", status: "warning", note: "Limited collaboration with design systems." },
    ],
  },
  {
    id: 2,
    name: "Paul Ladern",
    company: "Meta",
    role: "Senior",
    score: 83,
    status: "To Review",
    mandatory: [
      { id: "ts", status: "fail", note: "There is no mention of TypeScript in the resume." },
      { id: "aws", status: "pass", note: "Built services on AWS Lambda." },
      { id: "lead", status: "warning", note: "Team leadership focused on mentoring interns." },
      { id: "sec", status: "pending", note: "Security clearance check pending." },
    ],
    preferred: [
      { id: "tailwind", status: "pass", note: "TailwindCSS used in personal side projects." },
      { id: "nuxt", status: "warning", note: "Vue 3 projects listed; needs Nuxt confirmation." },
      { id: "ci", status: "pass", note: "Implemented CI pipelines with CircleCI." },
      { id: "design", status: "warning", note: "Limited collaboration with design systems." },
    ],
  },
  {
    id: 3,
    name: "Kevin Picus",
    company: "Expedia",
    role: "Senior",
    score: 73,
    status: "To Review",
    mandatory: [
      { id: "ts", status: "fail", note: "TypeScript experience under required level." },
      { id: "aws", status: "warning", note: "AWS usage limited to ECS deployments." },
      { id: "lead", status: "warning", note: "Project leadership limited to consulting engagements." },
      { id: "sec", status: "pending", note: "Security clearance check pending." },
    ],
    preferred: [
      { id: "tailwind", status: "warning", note: "CSS utilities mentioned but not Tailwind specifically." },
      { id: "nuxt", status: "warning", note: "Vue 2 expertise; Nuxt not confirmed." },
      { id: "ci", status: "pass", note: "Maintains CI/CD pipelines using GitLab CI." },
      { id: "design", status: "warning", note: "Limited collaboration with design systems." },
    ],
  },
];
</script>
