<template>
  <div class="h-[100vh]">
    <UContainer class="flex flex-col gap-10 py-10">
      <!-- stepper -->
      <div>
        <CustomStepper :steps="steps" :current-step="currentStep" />
      </div>
      <UDivider label="stepper" />
      <!-- marker array -->
      <div>
        <MarkerArray
          @output="handleOutput"
          :markerList="markerList"
          ref="markerComponentRef"
        />
        <!-- {{ markerList }} -->
        <div
          v-for="(value, index) in markerList"
          :key="index"
          class="flex justify-between space-y-2"
        >
          <div class="flex gap-2">
            <div
              class="w-6 h-6 rounded-full bg-[#2969E3] flex items-center justify-center text-white"
            >
              {{ index + 1 }}
            </div>
            <div>
              {{ value.description }}
            </div>
          </div>

          <div class="flex gap-4">
            <UButton @click="editMarker(value)">Edit</UButton>
            <UButton @click="removeMarker(value.id)">Delete</UButton>
          </div>
        </div>
      </div>
      <UDivider label="marker array" />
      <!-- upload -->
      <div>
        <UploadFile @change="handleFileChange" :files="files" />
      </div>
      <UDivider label="upload" />
      <!-- invoice type -->
      <div>
        <InvoiceType />
      </div>
      <UDivider label="detail" />
      <!-- table -->
      <div class="w-full overflow-x-auto">
        <Table
          fixedColumn
          :row="tableData"
          :columns="columns"
          :page="paginate.currentPage"
          :page-count="paginate.perPage"
          :page-total="paginate.totalPage"
          :slots="['name-data', 'age-data']"
        >
          <template #name-data="{ data }">
            <div class="bg-red-500">
              {{ data.name }}
            </div>
          </template>
          <template #age-data="{ data }">
            <div class="bg-yellow-500">
              {{ data.age }}
            </div>
          </template>
        </Table>
      </div>
      <UDivider label="table" />
      <!-- single -->
      <!-- <div class="flex flex-col items-center justify-center">
        <div class="relative inline-block">
          <img
            @click="placeMarker"
            src="https://media.istockphoto.com/id/1363086064/vector/usa-map-with-divided-states-on-a-transparent-background.jpg?s=612x612&w=0&k=20&c=rtw_UwKtPozK_QaNwOUHHuQMLXHvTU5mgK5hE7fZXyw="
            alt="Sample Image"
          />
          <div
            @click="removeMarker"
            v-show="isFirstMark"
            id="marker"
            class="absolute w-4 h-4 bg-red-500 rounded-full -translate-x-1/2 -translate-y-1/2"
          ></div>
        </div>
      </div>
      <UDivider label="single marker" /> -->
    </UContainer>
  </div>
</template>

<script setup>
import MarkerArray from "@/components/markerArray.vue";
import InvoiceType from "@/components/invoiceType.vue";
import CustomStepper from "@/components/stepper.vue";
import UploadFile from "@/components/upload.vue";
import Table from "~/components/table.vue";
definePageMeta({
  middleware: "auth",
});
const files = ref([
  {
    file: null,
    preview: "https://img.youtube.com/vi/ZG_W5qjtEqQ/maxresdefault.jpg",
  },
  {
    file: null,
    preview: "https://img.youtube.com/vi/KDorKy-13ak/maxresdefault.jpg",
  },
  {
    file: null,
    preview: "https://img.youtube.com/vi/9NdOBcOjw2s/maxresdefault.jpg",
  },
]);
const markerComponentRef = ref(null);
const token = useCookie("token");
const paginate = reactive({
  perPage: 10,
  currentPage: 2,
  totalPage: 5,
});
const isFirstMark = ref(false);
const xPercent = ref(90.5957164250911);
const yPercent = ref(82.43383170383794);

const steps = [
  {
    name: "แจ้งปัญหา",
    icon: "material-symbols:build-rounded",
    detail: "ช่างพบปัญหาจาก การซ่อมบำรุง / ล้าง",
  },
  {
    name: "กำลังดำเนินการ",
    icon: "material-symbols:warning-rounded",
    detail: "เจ้าหน้าที่รับทราบปัญหาแล้ว กำลังดำเนินการ",
  },
  {
    name: "เสร็จแล้ว",
    icon: "material-symbols:check-circle-rounded",
    detail: "เจ้าหน้าที่ดำเนินการ แก้ไขปัญหาแล้ว",
  },
];

const currentStep = ref(1);
const markerList = ref([
  // {
  //   id: 1739953778520,
  //   x: 26.225490196078432,
  //   y: 84.14376321353065,
  //   category: "",
  //   location: "",
  //   damageCode: "",
  //   repairCode: "",
  //   description: "dsada",
  //   images: [
  //     {
  //       file: "[object File]",
  //       preview:
  //         "blob:http://localhost:3000/a7b1360e-82e6-4d3a-816e-bf9d0d115c75",
  //     },
  //   ],
  //   isEdit: false,
  // },
  // {
  //   id: 1739953783021,
  //   x: 67.8921568627451,
  //   y: 42.28329809725159,
  //   category: null,
  //   location: null,
  //   damageCode: null,
  //   repairCode: null,
  //   description: "dsadsa",
  //   images: [
  //     {
  //       file: "[object File]",
  //       preview:
  //         "blob:http://localhost:3000/047c71fe-061d-4b64-bdd0-1863a9203e84",
  //     },
  //   ],
  //   isEdit: false,
  // },
]);

//emit change
const handleOutput = (value) => {
  console.log("Test emit", value);

  markerList.value = value;
};
//upload file
const handleFileChange = (value) => {
  console.log("Files changed:", value);
};

function removeMarker(id) {
  console.log("Parent removing marker with ID:", id);
  if (
    markerComponentRef.value &&
    typeof markerComponentRef.value.removeMarkerArray === "function"
  ) {
    markerComponentRef.value.removeMarkerArray(id);
  } else {
    console.error("Cannot access child component method");
  }
}
function editMarker(marker) {
  console.log("Parent editing marker:", marker);
  if (
    markerComponentRef.value &&
    typeof markerComponentRef.value.editMarker === "function"
  ) {
    markerComponentRef.value.editMarker(marker);
  } else {
    console.error("Cannot access child component method");
  }
}

//marker single
// function removeMarker() {
//   isFirstMark.value = false;
//   xPercent.value = null;
//   yPercent.value = null;
// }
// function placeMarker(event) {
//   isFirstMark.value = true;
//   const img = event.currentTarget;
//   const rect = img.getBoundingClientRect();

//   // Calculate position relative to the actual image size and its current displayed size
//   xPercent.value = ((event.clientX - rect.left) / rect.width) * 100;
//   yPercent.value = ((event.clientY - rect.top) / rect.height) * 100;

//   console.log("clicked position %:", xPercent.value, yPercent.value);

//   // Update marker position using Percentages relative to image
//   let marker = document.getElementById("marker");
//   marker.style.left = `${xPercent.value}%`;
//   marker.style.top = `${yPercent.value}%`;
// }

onMounted(() => {
  if (token.value) {
    //TODO integrate api get profile
    console.log("token", token.value);
  }
  //set marker position when have x and y perecent
  // let marker = document.getElementById("marker");
  // if (xPercent.value !== null && yPercent.value !== null) {
  //   isFirstMark.value = true;
  //   marker.style.left = `${xPercent.value}%`;
  //   marker.style.top = `${yPercent.value}%`;
  // }
});
// Expanded sample data with more entries and columns
const tableData = ref([
  {
    name: "John Doe",
    age: 30,
    email: "john@example.com",
    status: "Active",
    2: "Data 2-AData 2-AData 2-A",
    3: "Data 3-AData 2-AData 2-A",
    4: "Data 4-AData 2-AData 2-A",
    5: "Data 5-AData 2-AData 2-AData 2-A",
    6: "Data 6-AData 2-AData 2-AData 2-A",
    7: "Data 7-AData 2-AData 2-A",
    8: "Data 7-AData 2-A",
    9: "Data 7-Data 2-AA",
    10: "Data 7Data 2-A-A",
    11: "Data Data 2-A7-A",
    12: "Data 7Data 2-AData 2-A-A",
    13: "DataData 2-A 7-A",
  },
  {
    name: "John Doe",
    age: 30,
    email: "john@example.com",
    status: "Active",
    2: "Data 2-AData 2-AData 2-A",
    3: "Data 3-AData 2-AData 2-A",
    4: "Data 4-AData 2-AData 2-A",
    5: "Data 5-AData 2-AData 2-AData 2-A",
    6: "Data 6-AData 2-AData 2-AData 2-A",
    7: "Data 7-AData 2-AData 2-A",
    8: "Data 7-AData 2-A",
    9: "Data 7-Data 2-AA",
    10: "Data 7Data 2-A-A",
    11: "Data Data 2-A7-A",
    12: "Data 7Data 2-AData 2-A-A",
    13: "DataData 2-A 7-A",
  },
  {
    name: "John Doe",
    age: 30,
    email: "john@example.com",
    status: "Active",
    2: "Data 2-AData 2-AData 2-A",
    3: "Data 3-AData 2-AData 2-A",
    4: "Data 4-AData 2-AData 2-A",
    5: "Data 5-AData 2-AData 2-AData 2-A",
    6: "Data 6-AData 2-AData 2-AData 2-A",
    7: "Data 7-AData 2-AData 2-A",
    8: "Data 7-AData 2-A",
    9: "Data 7-Data 2-AA",
    10: "Data 7Data 2-A-A",
    11: "Data Data 2-A7-A",
    12: "Data 7Data 2-AData 2-A-A",
    13: "DataData 2-A 7-A",
  },
  {
    name: "John Doe",
    age: 30,
    email: "john@example.com",
    status: "Active",
    2: "Data 2-AData 2-AData 2-A",
    3: "Data 3-AData 2-AData 2-A",
    4: "Data 4-AData 2-AData 2-A",
    5: "Data 5-AData 2-AData 2-AData 2-A",
    6: "Data 6-AData 2-AData 2-AData 2-A",
    7: "Data 7-AData 2-AData 2-A",
    8: "Data 7-AData 2-A",
    9: "Data 7-Data 2-AA",
    10: "Data 7Data 2-A-A",
    11: "Data Data 2-A7-A",
    12: "Data 7Data 2-AData 2-A-A",
    13: "DataData 2-A 7-A",
  },
  {
    name: "Jane Smith",
    age: 25,
    email: "jane@example.com",
    status: "Inactive",
    2: "Data 2-B",
    3: "Data 3-B",
    4: "Data 4-B",
    5: "Data 5-B",
    6: "Data 6-B",
    7: "Data 7-B",
    8: "Data 7-A",
    9: "Data 7-A",
    10: "Data 7-A",
    11: "Data 7-A",
    12: "Data 7-A",
    13: "Data 7-A",
  },
  // ... [Previous data entries remain the same]
]);

// Define columns with the first column being sticky
const columns = ref([
  {
    key: "name",
    label: "Name",
    sortable: true,
    class: "sticky left-0 bg-white z-10",
  },
  {
    key: "age",
    label: "Age",
    sortable: true,
    class: "sticky left-[100px] bg-white z-10",
  },
  {
    key: "email",
    label: "Email",
  },
  {
    key: "status",
    label: "Status",
  },
  {
    key: "2",
    label: "Column 2",
  },
  {
    key: "3",
    label: "Column 3",
  },
  {
    key: "4",
    label: "Column 4",
  },
  {
    key: "5",
    label: "Column 5",
  },
  {
    key: "6",
    label: "Column 6",
  },
  {
    key: "7",
    label: "Column 7",
  },
  {
    key: "8",
    label: "Column 8",
  },
  {
    key: "9",
    label: "Column 9",
  },
  {
    key: "10",
    label: "Column 10",
  },
  {
    key: "11",
    label: "Column 11",
  },
  {
    key: "12",
    label: "Column 12",
  },
  {
    key: "13",
    label: "Column 13",
  },
]);
</script>

<style></style>
