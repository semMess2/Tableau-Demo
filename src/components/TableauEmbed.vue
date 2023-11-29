<template>
  <v-container>
    <div>
      <v-row justify="center">
        <tableau-viz
            id="tableauViz"
            src="https://public.tableau.com/views/Superstore_embedded_800x800/Overview"
            hide-tabs
        >
        </tableau-viz>
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

          <v-btn class="button" @click="filterState" height="40px" icon>
            <v-icon>mdi-magnify</v-icon>
          </v-btn>

          <v-btn class="button" @click="clearState" height="40px">
            ClearState
          </v-btn>

          <v-btn class="button" @click="unDo" height="40px">
            UnDo
          </v-btn>

        </v-col>
      </v-row>
    </div>
  </v-container>

</template>
<script>

export default {
  data() {
    return {
      inputValue: '',
      tableauViz: null,
      url: "https://public.tableau.com/views/Superstore_24/Overview",
      options: {
        hideTabs: false,
      }
    };
  },

  methods: {

    async filterState() {
      let viz = document.getElementById("tableauViz");
      let dataFilter = this.inputValue
      console.log(viz);
      let sheet = viz.workbook.activeSheet;
      const saleMap = sheet.worksheets.find((ws) => ws.name === "SaleMap");
      saleMap.applyFilterAsync("State", [dataFilter], "replace");
    },

    async clearState() {
      let viz = document.getElementById("tableauViz");
      let sheet = viz.workbook.activeSheet;
      const saleMap = sheet.worksheets.find((ws) => ws.name === "SaleMap");
      saleMap.clearFilterAsync("State");
    },

    async unDo() {
      let viz = document.getElementById("tableauViz");
      viz.undoAsync();
    }

  },

};
</script>
<style>
.button {
  margin-left: 15px;
}
</style>