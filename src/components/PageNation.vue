<script setup>
import { computed } from "vue";
import { useStore } from "vuex";
import axios from "axios";

const store = useStore();
const pageInfo = computed(() => store.state.info);
const currentPage = computed(() => store.state.currentPage);

async function prevPage() {
  const prevURL = pageInfo.value.prev;
  if(prevURL){
    const data = await axios.get(prevURL);
    store.commit("setState", data.data);
    store.commit("setCurrentPage", -1);
  } else return;
}

async function nextPage() {
  const nextURL = pageInfo.value.next;
  if(nextURL){
    const data = await axios.get(nextURL);
    store.commit("setState", data.data);
    store.commit("setCurrentPage", 1);
  } else return;
}

</script>
<template>
  <div class="flex flex-col items-center">
    <div class="inline-flex mt-2 xs:mt-0">
      <!-- Buttons -->
      <button @click="prevPage()" class="flex items-center justify-center px-3 h-8 text-sm font-medium text-white bg-gray-800 rounded-s hover:bg-gray-900 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white">
          <svg class="w-3.5 h-3.5 me-2 rtl:rotate-180" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 10">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 5H1m0 0 4 4M1 5l4-4"/>
          </svg>
          Prev
      </button>
      <div class="font-Akrobat text-white text-[20px] mx-[2rem]">{{ currentPage }} / {{ pageInfo.pages }}</div>
      <button @click="nextPage()" class="flex items-center justify-center px-3 h-8 text-sm font-medium text-white bg-gray-800 border-0 border-s border-gray-700 rounded-e hover:bg-gray-900 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white">
          Next
          <svg class="w-3.5 h-3.5 ms-2 rtl:rotate-180" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 10">
          <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M1 5h12m0 0L9 1m4 4L9 9"/>
        </svg>
      </button>
    </div>
  </div>
</template>
<style scoped></style>