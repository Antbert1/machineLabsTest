<template>
  <v-data-table :columns="columns" :rows="campaigns"
  :per-page="4" name="max" />
</template>

<script>
  import VDataTable from "./components/tables/VDataTable";
  import { format } from "date-fns";
  
  export default {
    components: {
      VDataTable,
    },
    data() {
      return {
        columns: [
          {
            dataKey: "code",
            name: "Code",
            align: "left",
          },
          {
            dataKey: "startDate",
            name: "Start Date",
            align: "left",
            formatValue(value) {
              return format(value, "do MMMM yyyy");
            },
          },
          {
            dataKey: "sales",
            name: "Sales",
            align: "right",
            formatValue(value) {
              return '£' + value.toLocaleString("en-GB");
            },
          },
        ],
        campaigns: [
          {
            code: "CAM1",
            startDate: new Date(2019, 2, 15),
            sales: 3209,
          },
          {
            code: "CAM2",
            startDate: new Date(2019, 1, 15),
            sales: 42469,
          },
          {
            code: "CAM3",
            startDate: new Date(2019, 2, 10),
            sales: 469,
          },
          {
            code: "CAM4",
            startDate: new Date(2019, 2, 9),
            sales: 11203,
          },
          {
            code: "CAM5",
            startDate: new Date(2019, 0, 1),
            sales: 123,
          },
          {
            code: "CAM6",
            startDate: new Date(2019, 0, 15),
            sales: 0,
          },
          {
            code: "CAM7",
            startDate: new Date(2019, 0, 10),
            sales: 8096,
          },
        ],
      }
    }
  }
</script>
