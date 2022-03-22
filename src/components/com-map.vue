<template>
  <div
    class="com-map_contianer"
    v-loading="loading"
    element-loading-text="拼命加载中"
    element-loading-spinner="el-icon-loading"
    element-loading-background="rgba(0, 0, 0, 0.5)"
  >
    <header>
      <div class="left">
        <h1>{{ title || "客户数据地图" }}</h1>
      </div>
    </header>
    <section style="flex: none">
      <div class="map" ref="map-box"></div>
    </section>
  </div>
</template>
<script>
import echarts from "echarts";
import "echarts/extension/bmap/bmap";
import styleJson from "@/common/map-style";
const BMap = window.BMap;
const symbol =
  "image://data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABUAAAAVCAYAAACpF6WWAAAACXBIWXMAAAsTAAALEwEAmpwYAAAFwmlUWHRYTUw6Y29tLmFkb2JlLnhtcAAAAAAAPD94cGFja2V0IGJlZ2luPSLvu78iIGlkPSJXNU0wTXBDZWhpSHpyZVN6TlRjemtjOWQiPz4gPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyIgeDp4bXB0az0iQWRvYmUgWE1QIENvcmUgNS42LWMxNDIgNzkuMTYwOTI0LCAyMDE3LzA3LzEzLTAxOjA2OjM5ICAgICAgICAiPiA8cmRmOlJERiB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPiA8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtbG5zOnhtcE1NPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvbW0vIiB4bWxuczpzdEV2dD0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL3NUeXBlL1Jlc291cmNlRXZlbnQjIiB4bWxuczpkYz0iaHR0cDovL3B1cmwub3JnL2RjL2VsZW1lbnRzLzEuMS8iIHhtbG5zOnBob3Rvc2hvcD0iaHR0cDovL25zLmFkb2JlLmNvbS9waG90b3Nob3AvMS4wLyIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ0MgKFdpbmRvd3MpIiB4bXA6Q3JlYXRlRGF0ZT0iMjAxOS0wMi0yNlQxNzowMzo0NSswODowMCIgeG1wOk1ldGFkYXRhRGF0ZT0iMjAxOS0wMi0yNlQxNzowMzo0NSswODowMCIgeG1wOk1vZGlmeURhdGU9IjIwMTktMDItMjZUMTc6MDM6NDUrMDg6MDAiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6MTcwNzI1ZDQtNGM1MS04YTRiLWFjNWEtZmM5NDQyYTdmZmJhIiB4bXBNTTpEb2N1bWVudElEPSJhZG9iZTpkb2NpZDpwaG90b3Nob3A6MmYxZjk2NTAtYzVlMS1iNTQ5LTgwMDEtNzhjNDg3ZDVlNDY5IiB4bXBNTTpPcmlnaW5hbERvY3VtZW50SUQ9InhtcC5kaWQ6NTMxNzE2MGMtN2Y2MC1jOTQ5LThkMDgtYmQzMzc0Njg5MDJjIiBkYzpmb3JtYXQ9ImltYWdlL3BuZyIgcGhvdG9zaG9wOkNvbG9yTW9kZT0iMyI+IDx4bXBNTTpIaXN0b3J5PiA8cmRmOlNlcT4gPHJkZjpsaSBzdEV2dDphY3Rpb249ImNyZWF0ZWQiIHN0RXZ0Omluc3RhbmNlSUQ9InhtcC5paWQ6NTMxNzE2MGMtN2Y2MC1jOTQ5LThkMDgtYmQzMzc0Njg5MDJjIiBzdEV2dDp3aGVuPSIyMDE5LTAyLTI2VDE3OjAzOjQ1KzA4OjAwIiBzdEV2dDpzb2Z0d2FyZUFnZW50PSJBZG9iZSBQaG90b3Nob3AgQ0MgKFdpbmRvd3MpIi8+IDxyZGY6bGkgc3RFdnQ6YWN0aW9uPSJzYXZlZCIgc3RFdnQ6aW5zdGFuY2VJRD0ieG1wLmlpZDoxNzA3MjVkNC00YzUxLThhNGItYWM1YS1mYzk0NDJhN2ZmYmEiIHN0RXZ0OndoZW49IjIwMTktMDItMjZUMTc6MDM6NDUrMDg6MDAiIHN0RXZ0OnNvZnR3YXJlQWdlbnQ9IkFkb2JlIFBob3Rvc2hvcCBDQyAoV2luZG93cykiIHN0RXZ0OmNoYW5nZWQ9Ii8iLz4gPC9yZGY6U2VxPiA8L3htcE1NOkhpc3Rvcnk+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+zU7dIQAAAr5JREFUOI21lU9IFFEcxz9v1N1Vd7dBkFDXDbKQVYLoYGbarcI6pGzdzYtGdTHs7DntECGkBz1UB0GTEFK7RWrkvTbTEtz1H1LMrq7trq6vw7zR1U1ag37wmDdvZj7v935/viOEr5pUkx0jAhCABmSpq7AeAztAUl2lGvss+wBQU2vZgB1wADYFR8ESQAyIA1spG6RDZcdIlrrPBZyAG3CpuU29lgA2gAiwrua/gG0F34OmeJgP6EAhm+sl4s2zmwQDNUSNIkCSr6/grZiQ9S2vyHMtAmuAAURTQoNgYE0AOUAeUAAUi6GuZhGYuq3XNeCuuoy9+CQgiS/NE/k4hjExgqys7ZONbf3AEvAT2FThkIKBNU3FrgAoEYOdrdmhQJPnXheO0tMHcwBAbGGGUHc72yfO9MnGtl5gUYFjwI6GmV074CYaLhVfPjSV3n98KBDA4S3Hc7cT8WmimWjYo+JvV6zdsnEALjHa49cvNWD3nDoUmArW624gxnr9mAl1KBaWpzbAyUKgxl115a9Ay9xVV2Hh80X2KmTXU6F2sBE1isykZGZ2TxlsGEXs1bKwoPtNiLSlo5qGWV9mp+TrK4ml7xl/HA99A6e+jNkUScVCwyzYBBDF65sKT7/NGBqZHgdvxSRmZyUUC03tEAMisr5l0Hg3THx5PgMv5zDev0Zeax3CbNmYYu16Ggci5B8LyvLzz0NP2oiH5g4FxoKzBJ8+QPou9JPrCmFqQdzyNItbD8HMmiljlbWzO4szWni4+2xywyDbqaPluZDJbeLBr/wYf8Hqy0fslJ3rl/72PmAVCCtoEkAIX7UlKDbM/teBQqLhUjHa42chUEvUOA5gCopvUl6/M6g8TBWULctTYYm0kr4czM6wpM+NqVx2dRJL+tbVkf8ofSJV+f9BpLfV2CfS4n/8TtKgRzXZMZK29hum3g75ohJ6hAAAAABJRU5ErkJggg==";
/**
 * @description 创建圆形覆盖物
 * @param {Number} lng 经度
 * @param {Number} lat 纬度
 * @param {Number} radius 半径（以 km 为单位）
 * @param {Number} opacity 透明度
 * @param {String} color 颜色
 * @return {Object} 圆形覆盖物
 */
function createCircleOverlay(
  lng,
  lat,
  radius = 30,
  opacity = 0.6,
  color = "#219dd0"
) {
  const point = new BMap.Point(lng, lat);
  return new BMap.Circle(point, radius * 1000, {
    strokeColor: color,
    strokeWeight: 2,
    strokeOpacity: opacity,
    fillColor: color,
    fillOpacity: opacity,
  });
}
export default {
  props: {
    center: {
      type: Array,
      default: () => [120.918964, 31.385231],
    },
    zoom: {
      type: Number,
      default: 11,
    },
    title: {
      type: String,
      required: true,
    },
    // 二级厂数据
    // [经度, 纬度, 简称, 全称, 地址, 电话, 法人]
    twoLevelData: {
      type: Array,
    },
    // 三级厂数据
    threeLevelData: {
      type: Array,
    },
    twoLevelTooltipFormatter: {
      type: Function,
      default: function ({ data }) {
        const name = data[3];
        const address = data[4];
        const phone = data[5];
        return `名称: ${name} <br> 地址: ${address} <br> 电话: ${phone}`;
      },
    },
    threeLevelTooltipFormatter: {
      type: Function,
      default: function ({ data }) {
        let address = data[3] || "";
        address = address.replace(/\^/g, "");
        return `企业名称: ${data[2]}<br>地址: ${address}<br>法人: ${data[4]}`;
      },
    },
    // 外圈半径
    logisticsRangeInnerRadius: {
      type: Number,
      default: 30,
    },
    // 内圈半径
    logisticsRangeOuterRadius: {
      type: Number,
      default: 60,
    },
  },
  data() {
    return {
      list: [],
      loading: false,
    };
  },
  mounted() {
    this.loading = true;
    this.initBmapChart();
  },
  methods: {
    // 数据赋值完成后必须调用
    dataCompleted() {
      this.$nextTick(() => {
        this.mapChart.setOption({
          series: [
            {
              id: "threeLevelPlant",
              name: "三级厂",
              data: this.threeLevelData,
            },
          ],
        });
        this.loading = false;
      });
    },
    /**
     * 初始化地图
     */
    initBmapChart() {
      const that = this;
      const mapChart = (that.mapChart = echarts.init(that.$refs["map-box"]));
      const option = {
        animation: false,
        tooltip: {
          trigger: "item",
        },
        bmap: {
          center: that.center,
          zoom: that.zoom,
          roam: true,
          mapStyle: {
            styleJson,
          },
        },
        series: [
          {
            id: "twoLevelPlant",
            name: "二级厂",
            type: "scatter",
            coordinateSystem: "bmap",
            symbol: "circle",
            symbolSize: 60,
            itemStyle: {
              color: "rgba(39, 146, 255, 0.9)",
            },
            label: {
              show: true,
              fontSize: 12,
              color: "#fff",
              formatter(params) {
                return params.data[2]; // 简称
              },
            },
            tooltip: {
              formatter: that.twoLevelTooltipFormatter,
            },
            data: that.twoLevelData,
            zlevel: 5,
            animation: false,
          },
          {
            id: "threeLevelPlant",
            name: "三级厂",
            type: "scatter",
            coordinateSystem: "bmap",
            symbol: "circle",
            symbolSize: 14,
            label: {
              show: false,
            },
            tooltip: {
              formatter: that.threeLevelTooltipFormatter,
            },
            data: that.threeLevelData,
            animation: false,
          },
        ],
      };
      mapChart.setOption(option);
      that.$nextTick(() => {
        that.mapSetting();
        that.initEvent();
      });
    },
    mapSetting() {
      const that = this;
      const map = that.mapChart.getModel().getComponent("bmap").getBMap();
      that.map = map;
      map.setMinZoom(10);
    },
    initEvent() {
      const that = this;
      const mapChart = that.mapChart;
      const map = that.map;
      mapChart.off("mouseover");
      mapChart.off("mouseout");
      // 二级厂鼠标悬浮 显示物流半径范围
      mapChart.on("mouseover", (event) => {
        if (event.seriesId === "twoLevelPlant") {
          const [lng, lat] = event.data;
          // 内部范围
          const circleInner = createCircleOverlay(
            lng,
            lat,
            that.logisticsRangeInnerRadius
          );
          // 外部范围
          const circleOuter = createCircleOverlay(
            lng,
            lat,
            that.logisticsRangeOuterRadius,
            0.2
          );
          if (that.circleInner) {
            map.removeOverlay(that.circleInner);
          }
          if (that.circleOuter) {
            map.removeOverlay(that.circleOuter);
          }
          that.circleInner = circleInner;
          that.circleOuter = circleOuter;
          map.addOverlay(circleInner);
          map.addOverlay(circleOuter);
        }
      });
      mapChart.on("mouseout", () => {
        map.removeOverlay(that.circleInner);
        map.removeOverlay(that.circleOuter);
      });
    },
  },
};
</script>
<style lang="scss" scoped>
.com-map_contianer {
  box-sizing: border-box;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  background: #001f40;
  color: #fff;
  font-family: "Microsoft YaHei";

  header {
    z-index: 3;
    position: relative;
    display: flex;
    justify-content: flex-start;
    background-image: url("~@/assets/images/header_bg_left.png");
    background-repeat: no-repeat;
    background-size: 100% 100%;
    padding: 6px 15px 20px;

    .left {
      width: 324px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;

      h1 {
        margin: 0;
        font-weight: 700;
        font-size: 36px;
        text-shadow: 2px 1px 1px #00bcd4;
      }
    }

    .fa {
      line-height: 68px;
    }
  }

  .map {
    z-index: 1;
    position: fixed !important;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
  }
}
</style>
