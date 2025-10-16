<template>
  <section class="rounded-[32px] border border-[#f5d87c] bg-white p-6 md:p-10 shadow-[0_24px_60px_rgba(15,23,42,0.08)]">
    <header class="flex flex-col gap-4 md:flex-row md:items-center md:justify-between">
      <h1 class="text-2xl font-semibold text-[#1f2a44]">Dashboard</h1>
      <div class="flex flex-wrap items-center gap-3">
        <button
          v-for="filter in filters"
          :key="filter.id"
          type="button"
          class="rounded-full border px-4 py-2 text-sm font-medium transition"
          :class="filter.active ? 'border-[#4b7ee8] bg-[#4b7ee8] text-white shadow-[0_12px_24px_rgba(75,126,232,0.25)]' : 'border-[#dde4f5] text-[#4b5c7c] hover:bg-[#f5f7ff]'"
        >
          <UIcon v-if="filter.icon" :name="filter.icon" class="mr-2 inline h-4 w-4" />
          {{ filter.label }}
        </button>
      </div>
    </header>

    <div class="mt-8 grid gap-4 md:grid-cols-3">
      <div
        v-for="metric in metrics"
        :key="metric.title"
        class="rounded-[24px] border border-[#e6ebf8] bg-[#f8f9ff] p-6 transition hover:border-[#d0daf5]"
      >
        <div class="flex items-center justify-between">
          <p class="text-sm font-semibold text-[#7a87a6]">
            {{ metric.title }}
          </p>
          <button class="text-xs text-[#9aa4bf]" type="button">
            <UIcon name="i-heroicons-question-mark-circle-20-solid" class="h-4 w-4" />
          </button>
        </div>
        <p class="mt-3 text-2xl font-semibold text-[#1f2a44]">
          {{ metric.value }}
        </p>
        <div class="mt-2 flex items-center gap-3 text-xs">
          <span :class="metric.trend > 0 ? 'text-[#1f8a4d]' : 'text-[#d14343]'">
            {{ metric.trend > 0 ? `+${metric.trend}%` : `${metric.trend}%` }}
          </span>
          <button type="button" class="text-[#4b7ee8] hover:underline">
            View details
          </button>
        </div>
      </div>
    </div>

    <section class="mt-10 space-y-5 rounded-[28px] border border-[#e6ebf8] bg-[#f8f9ff] p-6">
      <header class="flex flex-col gap-3 md:flex-row md:items-center md:justify-between">
        <h2 class="text-lg font-semibold text-[#1f2a44]">Statistics</h2>
        <div class="flex flex-wrap items-center gap-4">
          <div v-for="stat in statSummary" :key="stat.id" class="flex items-center gap-2 text-sm">
            <span class="h-2 w-2 rounded-full" :class="stat.color"></span>
            <div>
              <p class="font-semibold text-[#4b5c7c]">
                {{ stat.label }}
              </p>
              <p class="text-xs" :class="stat.delta > 0 ? 'text-[#1f8a4d]' : 'text-[#d14343]'">
                {{ stat.value }} <span class="ml-1">{{ stat.delta > 0 ? `+${stat.delta}%` : `${stat.delta}%` }}</span>
              </p>
            </div>
          </div>
        </div>
      </header>

      <div class="rounded-[24px] border border-[#d8e0f6] bg-white p-4">
        <DashboardLineChart />
      </div>

    
    </section>
  </section>
</template>

<script setup lang="ts">
import DashboardLineChart from "~/components/DashboardLineChart.client.vue";

const filters = [
  { id: "period", label: "Last 12 Month", icon: "i-heroicons-clock-20-solid", active: true },
  { id: "calendar", label: "Jan – Dec 2025", icon: "i-heroicons-calendar-days-20-solid", active: false },
  { id: "job", label: "All Jobs", icon: "i-heroicons-briefcase-20-solid", active: false },
];

const metrics = [
  { title: "Total Applicants", value: "1,000", trend: 2.4 },
  { title: "Total Passed", value: "4", trend: 3.4 },
  { title: "Total Rejected", value: "996", trend: -1.9 },
];

const statSummary = [
  { id: "monthly", label: "Monthly Apply", value: 123, delta: 3.4, color: "bg-[#b36df7]" },
  { id: "passed", label: "Avg. Passed", value: 2, delta: 2.5, color: "bg-[#3fb096]" },
  { id: "rejected", label: "Avg. Rejected", value: 234, delta: -7.2, color: "bg-[#f87171]" },
];
</script>
