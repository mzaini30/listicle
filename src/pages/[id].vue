<script setup="" lang="ts">
import { Ref, ref, watch } from "vue";
import { useRoute } from "vue-router";

const { params } = useRoute();

const judul = ref("");
const isi = ref("");
const semuaList: Ref<any[]> = ref([]); // id, judul, banyak
const belumDisave = ref(false);

if (localStorage.semuaList) {
  semuaList.value = JSON.parse(localStorage.semuaList);
  judul.value = semuaList.value.filter((x) => x.id === params.id)[0]?.judul;
}

if (localStorage[`tampil_${params.id}`]) {
  isi.value = localStorage[`tampil_${params.id}`];
}

function banyak() {
  return isi.value.split("\n").filter((x) => x).length;
}

function simpan() {
  localStorage[`tampil_${params.id}`] = isi.value;
  let banyakList = semuaList.value.filter((x) => x.id === params.id).length;
  if (banyakList > 0) {
    semuaList.value.forEach((x) => {
      if (x.id === params.id) {
        x.judul = judul.value;
        x.banyak = banyak();
      }
    });
  } else {
    semuaList.value = [
      {
        id: params.id,
        judul: judul.value,
        banyak: banyak(),
      },
      ...semuaList.value,
    ];
  }
  localStorage.semuaList = JSON.stringify(semuaList.value);
  belumDisave.value = false;
}

watch(
  () => judul.value,
  () => (belumDisave.value = true)
);

watch(
  () => isi.value,
  () => (belumDisave.value = true)
);
</script>

<template>
  <div class="p-3">
    <h1 class="mb-1 text-sm font-bold">
      {{ judul ? banyak() : null }} {{ judul || "..." }}
    </h1>
    <form action="" @submit.prevent="simpan">
      <input type="text" placeholder="Judul" required v-model="judul" />
      <textarea
        name=""
        id=""
        cols="30"
        rows="10"
        required
        v-model="isi"
        placeholder="- Pertama
- Kedua
- Ketiga"
      ></textarea>
      <div class="relative w-min">
        <div
          v-if="belumDisave"
          class="absolute -top-1 -right-1 w-3 aspect-square rounded-[50%] bg-red-500"
        ></div>
        <input
          type="submit"
          class="text-sm !bg-black rounded p-2 text-white cursor-pointer select-none"
          value="Simpan"
        />
      </div>
    </form>
  </div>
</template>

<style scoped="">
input:not([type="submit"]),
textarea {
  @apply focus:outline-none border-b block border-b-green-500 w-full mb-3;
}
textarea {
  @apply border border-green-500 p-1;
}
</style>
