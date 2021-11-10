<template>
  <div class="map">
    <h3>Карта офиса</h3>

    <div v-if="!isLoading" class="map-root">
      <MapSVG ref="svg" />
      <TableSVG ref="table" v-show="false" />
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import * as d3 from "d3";

import MapSVG from "@/assets/images/map.svg";
import TableSVG from "@/assets/images/workPlace.svg";

import tables from "@/assets/data/tables.json";
import legend from "@/assets/data/legend.json";

export default {
  components: {
    MapSVG,
    TableSVG,
  },

  data() {
    return {
      isLoading: false,

      svg: null,
      group: null,

      tables,
      tableSVG: null,
    };
  },

  methods: {
    drawTables() {
      this.tables.map((table) => {
        const svgTable = this.group
          .append("g")
          .attr("transform", `translate(${table.x} ${table.y}) scale(0.5)`)
          .attr("id", table._id)
          .classed("groupPlaces employer-place", true);

        svgTable
          .append("g")
          .attr("transform", `rotate(${table.rotate || 0})`)
          .attr("group_id", table.group_id)
          .html(this.tableSVG.html())
          .attr(
            "fill",
            legend.find((group) => group.group_id === table.group_id)?.color ??
              "transparent"
          );

        svgTable.on("click", this.onTableClick);
      });
    },
    onTableClick(e) {
      const employeeId = e.currentTarget.getAttribute("id");

      if (employeeId) {
        e.stopPropagation();
        this.$emit("showUserInfo", Number(employeeId));
      }
    },
  },

  mounted() {
    this.svg = d3.select(this.$refs.svg);
    this.group = this.svg.select("g");
    this.tableSVG = d3.select(this.$refs.table);

    if (this.group) {
      this.drawTables();
    } else {
      console.error("Error on drawing tables");
    }
  },
};
</script>

<style scoped>
.map {
  height: 100%;
  width: 100%;
  padding: 24px;
  overflow: hidden;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

.map-root {
  height: 100%;
  width: 100%;
  overflow: hidden;
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

::v-deep svg {
  height: 100%;
  width: 100%;
}

::v-deep .table {
  cursor: pointer;
}
</style>
