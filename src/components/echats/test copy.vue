<template>
  <div id="map-box"></div>
</template>

<script>
import mockdata from "@/common/scatter-data.json";
export default {
  data() {
    return {
      myChart:{}
    };
  },
  mounted() {
    console.log("mockdata",mockdata);
    var option = {
       animation: false,
        tooltip: {
          trigger: 'item'
        },
        bmap: {
          center: [120.918964, 31.385231],
          zoom: 8,
          roam: true,
          mapStyle: {
            
          }
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
            formatter: this.twoLevelTooltipFormatter,
          },
          data: [
            [
              112.711752,
              22.870747,
              "金利澳",
              "佛山市金利澳环保科技有限公司",
              "佛山市高明区明城镇城七路37号",
              "0757-89998618",
            ],
            [
              113.28504,
              22.885977,
              "御腾",
              "佛山市御腾环保科技有限公司",
              "佛山市顺德区伦教三洲振兴路6A号之一",
              "0757-29994088",
            ],
          ],
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
            formatter: this.threeLevelTooltipFormatter,
          },
          data: mockdata,
          animation: false,
        },
      ],
    };
    var hostmap = document.getElementById("map-box");
    var myChart = echarts.init(hostmap);
    this.myChart = myChart
    myChart.setOption(option);
    var map = myChart.getModel().getComponent("bmap").getBMap();
    map.setMinZoom(10);
    // map.disableDoubleClickZoom();
    map.centerAndZoom(new BMap.Point(112.988804, 23.033879), 11);  // 初始化地图
    window.onresize = myChart.resize; //图表自适应
//     function createCircleOverlay (lng, lat, radius = 30, opacity = 0.6, color = '#219dd0') {
//   const point = new BMap.Point(lng, lat)
//   return new BMap.Circle(point, radius * 1000, {
//     strokeColor: color,
//     strokeWeight: 2,
//     strokeOpacity: opacity,
//     fillColor: color,
//     fillOpacity: opacity
//   })
// }
// this.dataCompleted()
  },
  methods: {
    twoLevelTooltipFormatter({ data }) {
      const name = data[3];
      const address = data[4];
      const phone = data[5];
      return `名称: ${name} <br> 地址: ${address} <br> 电话: ${phone}`;
    },
    threeLevelTooltipFormatter({ data } = mockdata) {
      let address = data[3] || "";
      address = address.replace(/\^/g, "");
      return `企业名称: ${data[2]}<br>地址: ${address}<br>法人: ${data[4]}`;
    },
    dataCompleted () {
      this.$nextTick(() => {
        this.myChart.setOption({
          series: [{
            id: 'threeLevelPlant',
            name: '三级厂',
            data: mockdata
          }]
        })
      })
    },
  },
};
</script>

<style>
#map-box {
  width: 100vw;
  height: 100vh;
}
</style>