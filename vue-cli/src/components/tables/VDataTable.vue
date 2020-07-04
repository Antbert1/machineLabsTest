<template>
    <div>
        <table style="width:100%">
            <tr class="tableRow headingsRow">
                <td v-for="col in columns" :key="col.dataKey" :style="{textAlign: col.align}">{{ col.name }}</td>
            </tr>
            <tbody>
                <template v-for="row in rows.slice(startVal, endVal)">
                    <tr class="tableRow" :key="row.code">
                        <td :style="{textAlign: computedStyle(0)}">{{ row.code }}</td>
                        <td :style="{textAlign: computedStyle(1)}">{{ formatRow(row.startDate, 1) }}</td>
                        <td :style="{textAlign: computedStyle(2)}">{{ formatRow(row.sales, 2) }}</td>
                    </tr>
                </template>
            </tbody>
        </table>
        <button v-for="n in totalPages" :key="n" @click="page(n)">{{n}}</button>
    </div>
</template>

<script>
export default {
    props: ['columns', 'rows', 'per-page','name'],
    data: function() {
        return {
            startVal: 0
        }
    },
    computed: {
        totalPages: function() {
            //Calculate number of pages based on total length of rows and entries to show
            let totalRows = this.rows.length;
            let perPage = this.perPage;
            var total = Math.floor(totalRows/perPage);
            if (totalRows%perPage != 0) {
                total = total+1;
            }
            return total;
        },
        endVal: function() {
            //calculate end of splice, check full length
            let endVal = this.perPage
            //Check there are enough rows first 
            if (this.perPage < this.rows.length) {
                endVal = this.startVal + this.perPage;
            } else {
                endVal = this.rows.length;
            }
            return endVal
        }
    },
    methods: {
        formatRow(rowItem, i) {      
            return this.columns[i].formatValue(rowItem);
        },
        computedStyle: function(colIndex) {
            return this.columns[colIndex].align
        },
        page: function(n) {
            //Change page
            this.startVal = (n-1)*(this.perPage)
            console.log("next")
        }
    }
}
</script>

<style scoped>
</style>