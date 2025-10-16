<template>
  <div>
    <UTable :rows="row" :columns="columns" class="w-full" :class="fixedColumn ? 'fixedColumn' : ''">
      <!-- <template #name-data="{ row }">
          <div class="bg-red-500">
            {{ row.name }}
          </div>
        </template>
  
        <template #age-data="{ row }">
          <div class="bg-yellow-500">
            {{ row.age }}
          </div>
        </template> -->
      <template v-for="(slot, ind) in slots" #[slot]="{ row }">
        <div :key="ind">
          <slot :name="slot" :data="row"></slot>
        </div>
      </template>
    </UTable>
    <div
      class="relative flex items-center justify-center px-3 py-3.5 border-t border-gray-200 dark:border-gray-700"
    >
      <div v-if="isPaginate">
        <UPagination
          show-last
          show-first
          v-model="page"
          :page-count="pageCount"
          :total="pageTotal * pageCount"
          :per-page="pageCount"
          @click="changePage"
        />
      </div>
      <div v-if="limitSelect" class="absolute right-3 flex items-center gap-4">
        <USelect
          class="w-20"
          variant="outline"
          :options="[10, 25, 50, 100]"
          @change="onChangePerPage"
        />
        <div>Showing 1-{{ localPageCount }} of {{ pageTotal * pageCount }}</div>
      </div>
    </div>
  </div>
</template>

<script setup>
const emit = defineEmits(["onPageChange"]);
const page = ref(1);
const props = defineProps({
  slots: {
    type: Array,
    required: true,
    default: () => [],
  },
  row: {
    type: Array,
    required: true,
    default: () => [],
  },
  columns: {
    type: Array,
    required: true,
    default: () => [],
  },
  limitSelect: {
    type: Boolean,
    default: true,
  },
  isPaginate: {
    type: Boolean,
    default: true,
  },
  pageCount: {
    type: Number,
    default: 5,
  },
  pageTotal: {
    type: Number,
    default: 0,
  },
  fixedColumn: {
    type: Boolean,
    default: false,
  }
});
const localPageCount = ref(+props.pageCount || 0);

const onChangePerPage = (totalPage) => {
  page.value = 1;
  localPageCount.value = +totalPage;
  changePage();
};

const changePage = () => {
  console.log("test", page.value, localPageCount.value);

  emit("onPageChange", page.value, localPageCount.value);
};
</script>

<style>
/* fixed all table first 2 coloumn sticky (if not apply to class na krub) */
.fixedColumn table tbody tr td:nth-child(1) {
  position: sticky;
  left: 0;
  z-index: 10;
  background: white;
}
.fixedColumn table tbody tr td:nth-child(2) {
  position: sticky;
  left: 100px;
  z-index: 10;
  background: white;
}
</style>
