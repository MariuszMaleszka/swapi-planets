<template>
    <div class="q-pa-md">
        <q-table id="planetsTable" :grid="$q.screen.xs" bordered title="Planets" :rows="planets" :columns="columns"
            row-key="name" :filter="filter" :loading="isLoading" :sort-method="customSort" binary-state-sort>
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
        sort: (a: PlanetType, b: PlanetType) => a.populationNumber - b.populationNumber
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
        sortable: true
    },
    {
        name: 'gravity',
        align: 'left',
        label: 'Gravity',
        field: 'gravity',
        sortable: true,
        sort: (a: PlanetType, b: PlanetType) => a.gravityNumber - b.gravityNumber
    },
    {
        name: 'created',
        align: 'left',
        label: 'Created',
        field: 'created',
        sortable: true,
        format: formatDate
    },
    {
        name: 'url',
        align: 'right',
        label: 'Url',
        field: 'url'
    }
];
function customSort(rows: any, sortBy: any, descending: any) {
    const data = [...rows];
    if (sortBy) {
        data.sort((a, b) => {
            const x = descending ? b : a;
            const y = descending ? a : b;

            if (sortBy === 'name' || sortBy === 'created' || sortBy === 'climate') {
                // string sort
                return x[sortBy] > y[sortBy] ? 1 : x[sortBy] < y[sortBy] ? -1 : 0;
            } else {
                // numeric sort
                const xValue = parseFloat(x[sortBy]);
                const yValue = parseFloat(y[sortBy]);

                if (isNaN(xValue) && isNaN(yValue)) return 0;
                if (isNaN(xValue)) return 1;
                if (isNaN(yValue)) return -1;

                return xValue - yValue;
            }
        });
    }

    return data;
}

</script>
<style scoped lang="scss">
#planetsTable {
    @media (min-width: 600px) {
        min-width: 70dvw;
        max-width: 70dvw;
    }
}
</style>