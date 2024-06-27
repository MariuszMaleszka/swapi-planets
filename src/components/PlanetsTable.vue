<template>
    <div class="q-pa-md full-width">
        <q-table id="planetsTable" :grid="$q.screen.xs" bordered title="Planets" :rows="planets" :columns="columns"
            row-key="name" :filter="filter" :loading="isLoading" binary-state-sort>
            <template v-slot:top-right>
                <q-input debounce=" 300" v-model="filter" placeholder="Search">
                    <template v-slot:append>
                        <q-icon name="search" />
                    </template>
                </q-input>
            </template>
            <template v-slot:body-cell-url="props">
                <q-td :props="props">
                    <a class="text-white" :href="props.row.url" target="_blank" :title="props.row.name">{{
                        props.row.url }}</a>
                </q-td>
            </template>
            <template v-slot:loading>
                <q-inner-loading showing color="primary" />
            </template>
        </q-table>
    </div>
</template>

<script lang="ts" setup>
import { PlanetType } from 'src/types/PlanetType';
import { ref } from 'vue';
import { date } from 'quasar';
import { QTableProps } from 'quasar';

defineProps<{
    planets: PlanetType[],
    isLoading: boolean;
}>();

const filter = ref('');

const formatDate = (dateString: string) => {
    return date.formatDate(dateString, 'DD-MM-YYYY HH:mm:ss');
};

const columns: QTableProps['columns'] = [
    {
        name: 'name',
        required: true,
        label: 'Name',
        align: 'left',
        field: (row: PlanetType) => row.name,
        format: (val: string) => `${val}`,
        sortable: true,
    },
    {
        name: 'population',
        align: 'left',
        label: 'Population',
        field: 'population',
        sortable: true,
        sort: (_: string, _2: string, a: PlanetType, b: PlanetType) => a.populationNumber - b.populationNumber
    },
    {
        name: 'rotation_period',
        align: 'left',
        label: 'Rotation Period',
        field: 'rotation_period',
        sortable: true
    },
    {
        name: 'climate',
        align: 'left',
        label: 'Climate',
        field: 'climate',
        sortable: true,
        headerStyle: 'min-width: 250px; max-width: 300px'
    },
    {
        name: 'gravity',
        align: 'left',
        label: 'Gravity',
        field: 'gravity',
        sortable: true,
        sort: (_: string, _2: string, a: PlanetType, b: PlanetType) => a.gravityNumber - b.gravityNumber,

        headerStyle: 'min-width: 250px; max-width: 300px'
    },
    {
        name: 'created',
        align: 'left',
        label: 'Created',
        field: 'created',
        sortable: true,
        format: formatDate,
        sort: (a: string, b: string) => date.getDateDiff(a, b)
    },
    {
        name: 'url',
        align: 'right',
        label: 'Url',
        field: 'url'
    }
];


</script>
<style lang="scss">
#planetsTable {
    min-width: 100%;
    width: 100%;
    margin: 0 auto;

    @media (min-width: $breakpoint-md) {
        min-width: 70dvw;
        max-width: 70dvw;
    }

    @media (min-width: $breakpoint-lg) {
        min-width: 1280px;
        max-width: 1280px;
    }
}
</style>