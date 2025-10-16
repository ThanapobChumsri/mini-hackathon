<template>
    <div>
      <div>
        <UAccordion
          :items="itemsWithDefaultOpen"
          :ui="{
            wrapper: 'flex flex-col w-full',
            item: {
              padding: 'pt-0 pb-0',
            },
          }"
        >
          <template #default="{ item, open }">
            <UButton
              @click="handleClick(item)"
              color="gray"
              variant="ghost"
              class="border-b border-gray-200 hover:bg-gray-100 dark:border-gray-700"
              :class="
                  (page === item?.content[0].to && open)
                    ? 'bg-gray-100 text-black'
                    : ''
                "
              :ui="{ rounded: 'rounded-none', padding: { sm: 'p-5' } }"
            >
              <template #leading>
                <Icon :name="item.icon" class="text-2xl mr-3" />
              </template>
              <span class="truncate">{{ item.label }}</span>
             
              <template #trailing>
                <UIcon
                  v-if="item.content.length !== 1"
                  name="i-heroicons-chevron-right-20-solid"
                  class="w-5 h-5 ms-auto transform transition-transform duration-200"
                  :class="[open && 'rotate-90']"
                />
              </template>
            </UButton>
          </template>
          <template #item="{ item }">
            <a v-for="(menu, index) in item.content" :key="index" :href="menu.to">
              <p
                v-if="item.content.length !== 1"
                class="p-4 pr-5 pl-10 cursor-pointer"
                :class="
                  page === menu.to ||
                  (menu?.isCMS &&
                    secondPath == item.label.toLowerCase() &&
                    thirdPath == 'cms')
                    ? 'bg-[#2F3337] text-white'
                    : ''
                "
              >
                {{ menu.text }}
              </p>
              <p v-else class="w-full h-0 -mt-2"></p>
            </a>
          </template>
        </UAccordion>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        page: '',
        secondPath: '',
        thirdPath: '',
        fourthPath: '',
      }
    },
    props: {
      items: {
        type: Array,
        required: true,
      },
    },
    created() {
      this.test()
      this.getRoute()
    },
    methods: {
      test() {
        this.page = this.$route.path
        console.log('test', this.$route.fullPath)
        console.log('test', this.$route.path)
      },
      handleClick(item) {
        if (
          item.content &&
          item.content[0] &&
          item.content[0].to &&
          item.content.length === 1
        ) {
          this.$router.push(item.content[0].to)
        } else {
        }
      },
      getRoute() {
        const currentPath = this.$route.path
  
        const path = currentPath.split('/').filter((segment) => segment !== '')
  
        if (path.length >= 2) {
          this.secondPath = path[1]
          this.thirdPath = path[2]
          this.fourthPath = path[3]
        }
        return false
      },
    },
    computed: {
      itemsWithDefaultOpen() {
        return this.items.map((item) => ({
          ...item,
          defaultOpen: this.page.includes(item.page.toLowerCase()),
        }))
      },
    },
  }
  </script>
  
  <style></style>
  