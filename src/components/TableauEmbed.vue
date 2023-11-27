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
      inputValue: null
    };
  },
  methods: {
    filterState() {
      const dataFilter = this.inputValue;

      let viz = this.$refs.tableauViz;

      let sheet = viz.workbook.activeSheet;

      const saleMap = sheet.worksheets.find((ws) => ws.name === "SaleMap");

      saleMap.applyFilterAsync("State", [dataFilter], FilterUpdateType.Replace);
    }
  },
  mounted() {
    this.$nextTick(() => {
      const containerDiv = this.$refs.tableauViz;
      const url = "https://public.tableau.com/views/Superstore_embedded_800x800/Overview?:language=th-TH&:display_count=n&:origin=viz_share_link";
      const options = {
        hideTabs: false,
        onFirstInteractive: () => {
          console.log("Tableau viz has loaded.");
        },
      };
      const viz = new window.tableau.Viz(containerDiv, url, options);
      console.log(viz);
    });
  }
};
</script>
