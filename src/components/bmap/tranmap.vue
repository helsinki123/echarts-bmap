<template>
  <div>
    <div id="tranmap"></div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      startPoint: {
        x: 114,
        y: 22.5,
      },
      zoom: 15,
      styleJson: {
        styleJson: [
          {
            featureType: "water",
            elementType: "all",
            stylers: {
              color: "#d1d1d1",
            },
          },
          {
            featureType: "land",
            elementType: "all",
            stylers: {
              color: "#f3f3f3",
            },
          },
          {
            featureType: "railway",
            elementType: "all",
            stylers: {
              visibility: "off",
            },
          },
          {
            featureType: "highway",
            elementType: "all",
            stylers: {
              color: "#fdfdfd",
            },
          },
          {
            featureType: "highway",
            elementType: "labels",
            stylers: {
              visibility: "off",
            },
          },
          {
            featureType: "arterial",
            elementType: "geometry",
            stylers: {
              color: "#fefefe",
            },
          },
          {
            featureType: "arterial",
            elementType: "geometry.fill",
            stylers: {
              color: "#fefefe",
            },
          },
          {
            featureType: "poi",
            elementType: "all",
            stylers: {
              visibility: "off",
            },
          },
          {
            featureType: "green",
            elementType: "all",
            stylers: {
              visibility: "off",
            },
          },
          {
            featureType: "subway",
            elementType: "all",
            stylers: {
              visibility: "off",
            },
          },
          {
            featureType: "manmade",
            elementType: "all",
            stylers: {
              color: "#d1d1d1",
            },
          },
          {
            featureType: "local",
            elementType: "all",
            stylers: {
              color: "#d1d1d1",
            },
          },
          {
            featureType: "arterial",
            elementType: "labels",
            stylers: {
              visibility: "off",
            },
          },
          {
            featureType: "boundary",
            elementType: "all",
            stylers: {
              color: "#fefefe",
            },
          },
          {
            featureType: "building",
            elementType: "all",
            stylers: {
              color: "#d1d1d1",
            },
          },
          {
            featureType: "label",
            elementType: "labels.text.fill",
            stylers: {
              color: "#999999",
            },
          },
        ],
      },
    };
  },
  mounted() {
    // 百度地图API功能
    var map = new BMap.Map("tranmap"); // 创建Map实例
    map.centerAndZoom(
      new BMap.Point(this.startPoint.x, this.startPoint.y),
      this.zoom
    ); // 初始化地图,设置中心点坐标和地图级别
    map.setCurrentCity("深圳"); // 设置地图显示的城市 此项是必须设置的
    map.enableScrollWheelZoom(true); //开启鼠标滚轮缩放
    map.setMapStyle(this.styleJson); //地图样式
    var ctrl = new BMapLib.TrafficControl({
      showPanel: true, //是否显示路况提示面板
    });
    map.addControl(ctrl);
    ctrl.setAnchor(BMAP_ANCHOR_TOP_LEFT); //位置
    ctrl.showTraffic(); //默认开启路况
  },
};
</script>
<style scoped>
#tranmap {
  width: 100%;
  height: 100%;
  position: absolute;
  z-index: 100;
}
</style>