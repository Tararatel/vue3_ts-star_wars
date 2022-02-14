<template>
  <div style="text-align: center; margin: 2rem 0">
    <h1 class="title">Star Wars</h1>
  </div>
  <div v-if="loading">
    <p>loading</p>
  </div>
  <div v-else>
    <table>
      <thead>
        <tr>
          <td>name</td>
          <td>gender</td>
          <td>mass</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(people, index) in peoples.results" :key="index">
          <td>{{ people.name }}</td>
          <td>{{ people.gender }}</td>
          <td>{{ people.mass }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue';
import DataService from '@/services/DataService.ts';
import ResponseData from '@/types/ResponseData.ts';
import Peoples from '@/types/Peoples.ts';
export default defineComponent({
  setup() {
    const loading = ref(true as Boolean);
    const peoples = ref({} as Peoples);

    onMounted(() => getPeople());

    const getPeople = () => {
      DataService.getAll()
        .then((res: ResponseData) => {
          peoples.value = res.data;
          loading.value = false;
        })
        .catch((error: Error) => console.log(error));
    };
    return { loading, peoples };
  },
});
</script>
