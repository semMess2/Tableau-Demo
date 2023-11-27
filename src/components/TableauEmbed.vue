<template>
  <v-container>
    <div>
      <v-row justify="center">
        <div ref="tableauViz"></div>
      </v-row>
    </div>
    <div style="margin-top: 10px">
      <v-row justify="center">
        <v-col cols="3">
          <v-text-field
              v-model="inputValue"
              density="compact"
              variant="solo"
              label="Search Tableau"
              @click:append-inner="filterState"
          ></v-text-field>
        </v-col>
        <v-col cols="auto">
          <v-btn @click="filterState" height="40px" icon>
            <v-icon>mdi-magnify</v-icon>
          </v-btn>
        </v-col>
      </v-row>
    </div>
  </v-container>

</template>
<script>
import {FilterUpdateType} from 'tableau-api';

export default {
  data() {
    return {
      inputValue: ''
    };
  },
  methods: {

    filterState() {
      this.$nextTick(() => {
        this.filterState();
      });
    }
    // filterState() {
    //   this.$nextTick(() => {
    //     if (viz) {
    //       viz.addEventListener('vizReady', () => {
    //         const dataFilter = this.inputValue;
    //         let sheet = viz.workbook.activeSheet;
    //         const saleMap = sheet.worksheets.find((ws) => ws.name === "SaleMap");
    //         saleMap.applyFilterAsync("State", [dataFilter], FilterUpdateType.Replace);
    //       });
    //     }
    //   });
    // }
  },
  mounted() {
    this.$nextTick(() => {

      const containerDiv = this.$refs.tableauViz;
      const url = "https://public.tableau.com/views/Superstore_24/Overview";
      const options = {
        hideTabs: false,
        onFirstInteractive: () => {
          console.log("Tableau viz has loaded.");
          // let sheet = viz.getWorkbook().getActiveSheet();
          // console.log(sheet)
          // const saleMap = sheet.getWorksheets().find(ws => ws.name === "SaleMap");
          // saleMap.applyFilterAsync("State", ["Washington"], FilterUpdateType.Replace);
          // console.log(sheet)
        },
      };
      const viz = new window.tableau.Viz(containerDiv, url, options);
      console.log(viz);

      this.filterState = () => {
        let sheet = viz.getWorkbook().getActiveSheet();
        console.log(sheet)
        // const saleMap = sheet.getWorksheets();
        // console.log(saleMap)
        const saleMap = sheet.getWorksheets().find(ws => ws.name === "SaleMap");
        console.log("saleMap"+saleMap)
        const dataFilter = this.inputValue;
        console.log(dataFilter)
        const fieldExists = saleMap.applyFilterAsync("State", [dataFilter], FilterUpdateType.Replace);
        console.log(fieldExists)
      };
    });

  }
};
</script>
