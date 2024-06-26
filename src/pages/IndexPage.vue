<template>
  <q-layout>
    <q-page-container>
      <q-img class="main_bg" mode="cover" src="/src/assets/vader_bg.jpg" />
      <q-page class="column items-center justify-evenly">
        <PlanetsTable :planets="planetsData" :isLoading="isLoadingData" />
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useQuasar } from 'quasar';
import { api } from 'src/boot/axios';
import PlanetsTable from 'components/PlanetsTable.vue';
import { PlanetType } from 'src/types/PlanetType';

const $q = useQuasar();
const isLoadingData = ref(false);
const planetsData = ref<PlanetType[]>([]);

onMounted(() => {
  loadData();
});

async function loadData() {
  isLoadingData.value = true;
  try {
    const response = await api.get('https://swapi.dev/api/planets');
    const results = response.data.results;
    // Populate 
    planetsData.value = results.map((planet: PlanetType) => ({
      name: planet.name,
      population: planet.population,
      populationNumber: isNaN(parseInt(planet.population)) ? Number.NEGATIVE_INFINITY : parseInt(planet.population),
      rotation_period: planet.rotation_period,
      climate: planet.climate,
      gravity: planet.gravity,
      gravityNumber: isNaN(parseFloat(planet.gravity)) ? Number.NEGATIVE_INFINITY : parseFloat(planet.gravity),
      created: planet.created,
      url: planet.url
    })) as PlanetType[];
  } catch (error) {
    console.error(error);
    $q.notify({
      color: 'negative',
      position: 'top',
      message: 'Loading failed',
      icon: 'report_problem'
    });
  } finally {
    isLoadingData.value = false;
  }
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
