<template>
  <q-layout>
    <q-page-container>
      <q-img class="main_bg" mode="cover" src="/src/assets/vader_bg.jpg">
      </q-img>
      <q-page class="column items-center justify-evenly">
        <PlanetsTable :planets="planetsData" :isLoading="isLoadingData" />
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useQuasar, } from 'quasar';
import { api } from 'src/boot/axios';
import PlanetsTable from 'components/PlanetsTable.vue';
import { PlanetType } from 'src/types/PlanetType';
const $q = useQuasar();
const isLoadingData = ref(false);
const planetsData = ref<PlanetType[]>([]);

onMounted(() => {
  loadData();
});

function loadData() {
  isLoadingData.value = true; // Set loading state to true
  api.get('https://swapi.dev/api/planets')
    .then((response) => {
      const results = response.data.results;
      planetsData.value = results.map((planet: PlanetType) => ({
        name: planet.name,
        population: planet.population,
        rotation_period: planet.rotation_period,
        climate: planet.climate,
        gravity: planet.gravity,
        created: planet.created,
        url: planet.url
      })) as PlanetType[];
    })
    .catch(() => {
      $q.notify({
        color: 'negative',
        position: 'top',
        message: 'Loading failed',
        icon: 'report_problem'
      });
    })
    .then(() => {
      isLoadingData.value = false; // Set loading state to false after successful or failed API call
    });
}



</script>
<style scoped lang="scss">
.main_bg {
  position: fixed;
  height: 100dvh;
  width: 100dvw;
  object-position: center;

  &::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.808);
  }

}
</style>