<template>
<div>
<div id="aqi"></div>
</div>
</template>
<script>
import infor from '../../api/ajax'
import common from '../../api/common'
export default {
    mounted (){
 var worldMapContainer =common.getById(aqi);
//ecahts高度
  worldMapContainer.style.width=window.innerWidth+'px';
  worldMapContainer.style.height=((window.innerHeight)*0.9)+'px';
  var myChart = echarts.init(worldMapContainer);
  var lineStyle = {
    normal: {
        width: 1.5,
        opacity: 0.5
    }
};
  var option = {
  title: {
    text: 'Basic Radar Chart'
  },
  legend: {
    data: ['Allocated Budget', 'Actual Spending']
  },
  radar: {
    // shape: 'circle',
    indicator: [
      { name: 'Sales', max: 6500 },
      { name: 'Administration', max: 16000 },
      { name: 'Information Technology', max: 30000 },
      { name: 'Customer Support', max: 38000 },
      { name: 'Development', max: 52000 },
      { name: 'Marketing', max: 25000 }
    ]
  },
  series: [
    {
      name: 'Budget vs spending',
      type: 'radar',
      data: [
        {
          value: [4200, 3000, 20000, 35000, 50000, 18000],
          name: 'Allocated Budget'
        },
        {
          value: [5000, 14000, 28000, 26000, 42000, 21000],
          name: 'Actual Spending'
        }
      ]
    }
  ]
};
  myChart.setOption(option);
//窗口或框架被调整大小时发生。
window.onresize = function () {
    //重置容器高宽
    myChart.resize();
};    
    
    }
   
}
</script>
<style scoped>


</style>