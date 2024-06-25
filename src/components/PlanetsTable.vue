<template>
    <div class="q-pa-md">
        <q-table id="planetsTable" :grid="$q.screen.xs" bordered title="Planets" :rows="props.planets"
            :columns="columns" row-key="name" :filter="filter">
            <template v-slot:top-right>
                <q-input debounce="300" v-model="filter" placeholder="Search">
                    <template v-slot:append>
                        <q-icon name="search" />
                    </template>
                </q-input>
            </template>
        </q-table>
    </div>
</template>

<script lang="ts" setup>
import { PlanetType } from 'src/types/PlanetType';
import { ref } from 'vue';
const filter = ref('');
const props = defineProps<{
    planets: PlanetType[],
    isLoading: boolean;
}>();
console.log(props.isLoading);

interface Column {
    name: string;
    required?: boolean;
    label: string;
    align?: 'left' | 'center' | 'right';
    field: string | ((row: any) => any);
    format?: (val: string) => string;
    sortable?: boolean;
}
const columns: Column[] = [
    {
        name: 'name',
        required: true,
        label: 'Name',
        align: 'left',
        field: (row: PlanetType) => row.name,
        format: (val: string) => `${val}`,
        sortable: true
    },
    { name: 'population', align: 'center', label: 'Population', field: 'population', sortable: true },
    { name: 'rotation_period', label: 'Rotation Pperiod', field: 'rotation_period', sortable: true },
    { name: 'climate', label: 'Climate', field: 'climate', sortable: true },
    { name: 'gravity', label: 'Gravity', field: 'gravity', sortable: true },
    { name: 'created', label: 'Created', field: 'created', sortable: true },
    { name: 'url', label: 'Url', field: 'url' }
]





</script>
<style scoped lang="scss">
#planetsTable {
    @media (min-width: 600px) {
        min-width: 70dvw;
        max-width: 70dvw;
    }
}
</style>