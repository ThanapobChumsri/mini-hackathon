<template>
  <div class="relative">
    <canvas ref="canvasEl" class="w-full" height="260"></canvas>
  </div>
</template>

<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref, shallowRef } from "vue";

const canvasEl = ref<HTMLCanvasElement | null>(null);
const chartInstance = shallowRef<any>(null);

const labels = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"];

const datasets = [
  {
    label: "Monthly Apply",
    data: [0, 0, 0, 0, 150, 220, 240, 260, 280, 290, 300, 320],
    borderColor: "#b36df7",
    backgroundColor: "rgba(179, 109, 247, 0.15)",
    tension: 0.4,
    fill: true,
  },
  {
    label: "Avg. Passed",
    data: [0, 0, 0, 0, 50, 90, 120, 140, 160, 175, 180, 185],
    borderColor: "#3fb096",
    backgroundColor: "rgba(63, 176, 150, 0.15)",
    tension: 0.4,
    fill: true,
  },
  {
    label: "Avg. Rejected",
    data: [0, 0, 0, 0, 110, 160, 190, 210, 230, 240, 250, 255],
    borderColor: "#f59f5a",
    backgroundColor: "rgba(245, 159, 90, 0.15)",
    tension: 0.4,
    fill: true,
  },
];

const ensureChartJs = () =>
  new Promise<any>((resolve, reject) => {
    if (typeof window === "undefined") {
      return reject(new Error("Chart.js can only load on client"));
    }

    if ((window as any).Chart) {
      resolve((window as any).Chart);
      return;
    }

    const existing = document.querySelector<HTMLScriptElement>("script[data-dashboard-chart]");
    if (existing) {
      existing.addEventListener("load", () => resolve((window as any).Chart));
      existing.addEventListener("error", () => reject(new Error("Failed to load Chart.js")));
      return;
    }

    const script = document.createElement("script");
    script.src = "https://cdn.jsdelivr.net/npm/chart.js@4.4.4/dist/chart.umd.min.js";
    script.async = true;
    script.dataset.dashboardChart = "true";
    script.addEventListener("load", () => resolve((window as any).Chart));
    script.addEventListener("error", () => reject(new Error("Failed to load Chart.js")));
    document.head.appendChild(script);
  });

onMounted(async () => {
  try {
    const Chart = await ensureChartJs();
    if (!canvasEl.value) return;

    chartInstance.value = new (Chart as any)(canvasEl.value.getContext("2d"), {
      type: "line",
      data: {
        labels,
        datasets,
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        scales: {
          y: {
            beginAtZero: true,
            grid: {
              color: "#e5eaf9",
            },
            ticks: {
              color: "#7a87a6",
              font: { size: 11 },
            },
          },
          x: {
            grid: {
              display: false,
            },
            ticks: {
              color: "#7a87a6",
              font: { size: 11 },
            },
          },
        },
        plugins: {
          legend: {
            display: false,
          },
          tooltip: {
            backgroundColor: "#fff",
            titleColor: "#1f2a44",
            bodyColor: "#4b5c7c",
            borderWidth: 1,
            borderColor: "#dfe4f5",
            callbacks: {
              labelColor(context) {
                return {
                  borderColor: context.dataset.borderColor,
                  backgroundColor: context.dataset.borderColor,
                };
              },
            },
          },
        },
      },
    });
  } catch (error) {
    console.error("Failed to render dashboard chart:", error);
  }
});

onBeforeUnmount(() => {
  chartInstance.value?.destroy();
});
</script>
