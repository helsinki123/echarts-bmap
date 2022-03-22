<template>
  <ComMap ref="map" title="散点图示例" :center="[112.988804, 23.033879]" :two-level-data="twoLevelData" :three-level-data="threeLevelData"></ComMap>
</template>
<script>
import ComMap from '@/components/com-map'
import mockData from '@/common/scatter-data.json'
export default {
  components: {
    ComMap
  },
  data () {
    return {
      threeLevelData: [],
      twoLevelData: [
        [112.711752, 22.870747, '金利澳', '佛山市金利澳环保科技有限公司', '佛山市高明区明城镇城七路37号', '0757-89998618'],
        [113.28504, 22.885977, '御腾', '佛山市御腾环保科技有限公司', '佛山市顺德区伦教三洲振兴路6A号之一', '0757-29994088']
      ]
    }
  },
  mounted () {
    // mock async data
    // setTimeout(() => {
    //   this.threeLevelData = mockData
    //   this.$refs.map.dataCompleted()
    // }, 1000)
  }
}

</script>
