<template>
    <div class="container outerContainer">
        <div class="row">
            <div class="tableOuter" :style="{minHeight: minHeightCalc()}">
                <table>
                    <tr class="tableRow headingsRow">
                        <td v-for="(col, i) in columns" :key="col.dataKey" :style="{textAlign: col.align}">{{ col.name }} 
                            <img src="../../assets/sort.png" class="sortImg" @click="sortItems(i)" />
                        </td>
                    </tr>
                    <tbody>
                        <template v-for="(row, i) in rowsToUse.slice(startVal, endVal)">
                            <tr :key="row.code" :class="rowClasses(i)">
                                <td :style="{textAlign: computedStyle(0)}" class="boldText">{{ row.code }}</td>
                                <td :style="{textAlign: computedStyle(1)}">{{ formatRow(row.startDate, 1) }}</td>
                                <td :style="{textAlign: computedStyle(2)}">{{ formatRow(row.sales, 2) }}</td>
                            </tr>
                        </template>
                    </tbody>
                </table>
            
            </div>
        </div>
        <div class="buttonRow">
            <div class="buttons">
                <div v-for="n in totalPages" :key="n" @click="page(n)" :class="pageNumClass(n)">{{n}}</div>
            </div>
            <div class="totals">
                {{ totalPages }} pages
            </div>
            <div>
                ({{ rows.length }} results)
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['columns', 'rows', 'per-page','name'],
        data: function() {
            //startVal used for pagination, rowsToUse is a copy of rows that can be sorted and updated, asc to be toggled for sorting
            return {
                startVal: 0,
                rowsToUse: this.rows,
                asc: true
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
                //Check there are enough rows first. If not, just use the length of rows 
                if (this.perPage < this.rows.length) {
                    endVal = this.startVal + this.perPage;
                } else {
                    endVal = this.rows.length;
                }
                return endVal
            }
        },
        methods: {
            minHeightCalc: function() {
                //Calculate min height for table so that numbers row doesn't jump up on the last page. Using 65 as estimate of row height plus a bit
                var minHeight = 65*this.perPage;
                return minHeight+"px";
            },
            rowClasses: function(i) {
                //Give light or dark styles alternately on rows
                if (i%2==0) {
                    return {
                        dark: true,
                        tableRow: true
                    }
                } else {
                    return {
                        light: true,
                        tableRow: true
                    }
                }

            },
            pageNumClass: function(num) {
                //Check if last one for border 
                if (num == this.totalPages) {
                    //Check if current
                    if ((num-1)*this.perPage == this.startVal) {
                        return {
                            pageNum: true,
                            pageNumEnd: true,
                            active: true
                        }
                    } else {
                        return {
                            pageNum: true,
                            pageNumEnd: true
                        }
                    }
                    
                } else {
                    if ((num-1)*this.perPage == this.startVal) {
                        return {
                            pageNum: true,
                            active: true
                        }
                    } else {
                        return {
                            pageNum: true
                        }
                    }
                }
                
            },
            formatRow(rowItem, i) {   
                //Format required values   
                return this.columns[i].formatValue(rowItem);
            },
            computedStyle: function(colIndex) {
                //Use set alignment
                return this.columns[colIndex].align
            },
            page: function(n) {
                //Change page
                this.startVal = (n-1)*(this.perPage)
                console.log("next")
            },
            sortItems(index) {
                //Toggle an ascecnding variable. Use index to know which way to sort
                if (this.asc) {
                    this.rows.sort(dynamicSort(this.columns[index].dataKey)).reverse(); 
                } else {
                    this.rows.sort(dynamicSort(this.columns[index].dataKey)); 
                }
                this.asc = !this.asc;
                
            }
        }
    }

    function dynamicSort(property) {
        //External function that does sorting
        switch(property) {
            case "code":
                var sortOrder = 1;

                if(property[0] === "-") {
                    sortOrder = -1;
                    property = property.substr(1);
                }

                return function (a,b) {
                    if(sortOrder == -1){
                        return b[property].localeCompare(a[property], undefined, {numeric: true});
                    }else{
                        return a[property].localeCompare(b[property], undefined, {numeric: true});
                    }        
                }
                break;
            case "startDate": 
                return function(a,b) {
                    return new Date(b.startDate) - new Date(a.startDate);
                }
                break;
            case "sales":
                return function(a,b) {
                    return b.sales - a.sales;
                }
                break;
        }
    }
</script>

<style scoped>
    .outerContainer {
        padding-top: 20px;
    }
    .sortImg {
        height: 12px;
        cursor: pointer;
    }
    .headerRowItem {
        display: flex;
        align-items: center;
    }
    table {
        color: #5c6e7c;
    }
    td {
        padding-right: 30px;
        padding-top: 10px;
        padding-bottom: 10px;
        padding-left: 10px;
        min-width: 177px;
    }
    .tableOuter {
        background-color: #f6f7f9;
        padding: 20px;
    }
    .dark {
        background-color: #e6ecec;
    }
    .boldText {
        font-weight: bold;
    }
    .pageNum {
        border-left: 1px solid #e6ecec;
        border-top: 1px solid #e6ecec;
        border-bottom: 1px solid #e6ecec;
        cursor: pointer;
        min-width: 24px;
        min-height: 24px;
        text-align: center;
        background: white;
    }
    .pageNumEnd {
        border-right: 1px solid #e6ecec;
    }
    .buttonRow {
        display: flex;
        color: grey;
        padding-top: 20px;
    }
    .buttons {
        display: flex;
        padding-right: 20px;
        color: #5c6e7c;
        font-weight: bold;
    }
    .totals {
        padding-right: 10px;
    }
    .active {
        background-color: #e6ecec;
    }

    @media only screen and (max-width: 561px) {
        td {
            min-width: auto;
        }
    }
</style>