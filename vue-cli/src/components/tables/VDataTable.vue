<template>
    <div>
        <table style="width:100%">
            <tr class="tableRow headingsRow">
                <td v-for="col in columns" :key="col.dataKey" :style="{textAlign: col.align}">{{ col.name }}</td>
            </tr>
            <tbody>
                <template v-for="row in rows">
                    <tr class="tableRow" :key="row.code">
                        <td :style="{textAlign: computedStyle(0)}">{{ row.code }}</td>
                        <td :style="{textAlign: computedStyle(1)}">{{ formatRow(row.startDate, 1) }}</td>
                        <td :style="{textAlign: computedStyle(2)}">{{ formatRow(row.sales, 2) }}</td>
                    </tr>
                </template>
            </tbody>
        </table>
        <!-- <div v-for="col in columns" :key="col.dataKey">
            {{ col.name }}
        </div> -->

            
        <!-- <p>{{ name }}</p>
        <p> {{rows[0].code}}</p>
        <button @click="changeText">Change</button> -->
    </div>
</template>

<script>
export default {
    props: ['columns', 'rows', 'per-page','name'],
    data: function() {
        return {
            test: "test"
        }
    },
    methods: {
        formatRow(rowItem, i) {      
            return this.columns[i].formatValue(rowItem);
        },
        computedStyle: function(colIndex) {
            return this.columns[colIndex].align
        }
    }
}
</script>

<style scoped>
</style>