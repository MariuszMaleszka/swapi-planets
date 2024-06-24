<template>
  <q-layout>
    <q-page-container>
      <q-img class="main_bg" mode="cover" src="/src/assets/vader_bg.jpg">
      </q-img>
      <q-page class="column items-center justify-evenly">
        <PlanetsTable></PlanetsTable>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
// import { Todo, Meta } from 'components/models';
import PlanetsTable from 'components/PlanetsTable.vue'
import { ref } from 'vue';

// const todos = ref<Todo[]>([
//   {
//     id: 1,
//     content: 'ct1'
//   },
//   {
//     id: 2,
//     content: 'ct2'
//   },
//   {
//     id: 3,
//     content: 'ct3'
//   },
//   {
//     id: 4,
//     content: 'ct4'
//   },
//   {
//     id: 5,
//     content: 'ct5'
//   }
// ]);
// const meta = ref<Meta>({
//   totalCount: 1200
// });
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