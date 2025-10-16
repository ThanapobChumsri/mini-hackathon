<template>
  <div class="min-h-screen bg-[#eef2f9] flex">
    

    <main class="flex-1 px-4 py-10 md:px-10">
      <div class="mx-auto w-full max-w-6xl space-y-8">
        <header class="flex items-center justify-between">
          <div>
            <h1 class="text-2xl font-semibold text-[#1f2a44]">Jobs</h1>
            <p class="text-sm text-[#7a87a6]">Manage open roles and applicant pipelines.</p>
          </div>
          <button
            type="button"
            class="inline-flex items-center gap-2 rounded-full bg-[#4b7ee8] px-5 py-2 text-sm font-semibold text-white shadow-[0_12px_30px_rgba(75,126,232,0.35)] transition hover:bg-[#3f6fd4]"
            @click="goToCreateJob"
          >
            <UIcon name="i-heroicons-plus-20-solid" class="h-4 w-4" />
            Create Job
          </button>
        </header>

        <section
          class="rounded-[28px] border border-[#e0e6f4] bg-white p-6 shadow-[0_24px_60px_rgba(15,23,42,0.05)] md:p-8"
        >
          <div class="flex flex-col gap-4 pb-6 md:flex-row md:items-center md:justify-between">
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
                type="button"
                class="hidden items-center justify-center rounded-full border border-[#dae2f5] bg-white px-3 py-2 text-xs font-medium text-[#7a87a6] transition hover:bg-[#f8faff] md:inline-flex"
              >
                <UIcon name="i-heroicons-ellipsis-horizontal-20-solid" class="h-4 w-4" />
              </button>
            </div>

            <div class="flex items-center gap-2">
              <button
                type="button"
                class="inline-flex items-center gap-2 rounded-full border border-[#dae2f5] bg-white px-4 py-2 text-xs font-semibold text-[#4b5c7c] transition hover:bg-[#f3f6ff]"
              >
                <UIcon name="i-heroicons-funnel-20-solid" class="h-4 w-4" />
                Filter
              </button>
              <button
                type="button"
                class="inline-flex items-center gap-2 rounded-full border border-[#dae2f5] bg-white px-4 py-2 text-xs font-semibold text-[#4b5c7c] transition hover:bg-[#f3f6ff]"
              >
                <UIcon name="i-heroicons-bars-arrow-down-20-solid" class="h-4 w-4" />
                Sort
              </button>
            </div>
          </div>

          <div class="overflow-hidden rounded-3xl border border-[#e6ebf8]">
            <table class="min-w-full divide-y divide-[#e6ebf8]">
              <thead class="bg-[#f8f9ff] text-left text-xs font-semibold uppercase tracking-wider text-[#7a87a6]">
                <tr>
                  <th class="px-6 py-4">
                    <input type="checkbox" class="h-4 w-4 rounded border-[#ccd5ea] text-[#4b7ee8] focus:ring-[#4b7ee8]" />
                  </th>
                  <th class="px-4 py-4">Action</th>
                  <th class="px-4 py-4">Job Name</th>
                  <th class="px-4 py-4">Role</th>
                  <th class="px-4 py-4 text-right">Total Applicants</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-[#eef1fb] bg-white text-sm text-[#2f3c5a]">
                <tr
                  v-for="job in filteredJobs"
                  :key="job.id"
                  class="transition hover:bg-[#f4f6ff]"
                >
                  <td class="px-6 py-4">
                    <input type="checkbox" class="h-4 w-4 rounded border-[#ccd5ea] text-[#4b7ee8] focus:ring-[#4b7ee8]" />
                  </td>
                  <td class="px-4 py-4">
                    <div class="flex items-center gap-2">
                      <button
                        type="button"
                        class="flex h-8 w-8 items-center justify-center rounded-lg bg-[#4b7ee8] text-white shadow-[0_10px_20px_rgba(75,126,232,0.25)]"
                      >
                        <UIcon name="i-heroicons-pencil-square-16-solid" class="h-4 w-4" />
                      </button>
                      <button
                        type="button"
                        class="flex h-8 w-8 items-center justify-center rounded-lg border border-[#dbe2f3] text-[#4b5c7c] transition hover:bg-[#f1f4ff]"
                      >
                        <UIcon name="i-heroicons-user-group-16-solid" class="h-4 w-4" />
                      </button>
                    </div>
                  </td>
                  <td class="px-4 py-4 font-medium">{{ job.name }}</td>
                  <td class="px-4 py-4 text-[#66759a]">{{ job.role }}</td>
                  <td class="px-4 py-4 text-right font-semibold text-[#1f2a44]">
                    {{ job.totalApplicants }}
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </section>
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const search = ref("");

const sidebarItems = [
  { id: "dashboard", icon: "i-heroicons-home-20-solid" },
  { id: "jobs", icon: "i-heroicons-briefcase-20-solid" },
  { id: "candidates", icon: "i-heroicons-users-20-solid" },
  { id: "calendar", icon: "i-heroicons-calendar-days-20-solid" },
  { id: "settings", icon: "i-heroicons-cog-6-tooth-20-solid" },
];

const jobs = [
  { id: 1, name: "Senior Back-End Developer", role: "Senior", totalApplicants: 10 },
  { id: 2, name: "Junior Front-End Developer", role: "Junior", totalApplicants: 2 },
  { id: 3, name: "Lead Product Designer", role: "Lead", totalApplicants: 7 },
  { id: 4, name: "Data Analyst", role: "Mid-Level", totalApplicants: 4 },
  { id: 5, name: "UX Researcher", role: "Mid-Level", totalApplicants: 5 },
  { id: 6, name: "DevOps Engineer", role: "Senior", totalApplicants: 8 },
  { id: 7, name: "Mobile App Developer", role: "Junior", totalApplicants: 1 },
  { id: 8, name: "Systems Architect", role: "Senior", totalApplicants: 12 },
  { id: 9, name: "Quality Assurance Tester", role: "Mid-Level", totalApplicants: 3 },
  { id: 10, name: "Technical Project Manager", role: "Senior", totalApplicants: 6 },
];

const filteredJobs = computed(() => {
  if (!search.value.trim()) {
    return jobs;
  }
  const keyword = search.value.trim().toLowerCase();
  return jobs.filter((job) => job.name.toLowerCase().includes(keyword));
});

const goToCreateJob = () => {
  router.push("/Recruitment");
};
</script>
