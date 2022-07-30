<script setup="" lang="ts">
import Info from "../komponen/info.vue";
import { Ref, ref } from "vue";

const data: Ref<any[]> = ref([]);

if (localStorage.semuaList) {
  data.value = JSON.parse(localStorage.semuaList);
}
</script>

<template>
  <Info></Info>
  <div class="p-3">
    <div class="mb-3">
      <router-link
        :to="`/${Math.random().toString().replace('0.', '')}`"
        class="text-sm inline-block p-3 rounded bg-green-500 text-white select-none"
        >Tambah</router-link
      >
    </div>
    <div class="">
      <router-link
        v-for="x in data"
        class="block p-2 underline"
        :to="`/${x.id}`"
        >{{ x.banyak }} {{ x.judul }}</router-link
      >
    </div>
  </div>
</template>
