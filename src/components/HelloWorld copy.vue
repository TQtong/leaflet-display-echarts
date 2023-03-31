<!--
 * @Author: 2733707740@qq.com 2733707740@qq.com
 * @Date: 2023-03-31 16:01:26
 * @LastEditors: 2733707740@qq.com 2733707740@qq.com
 * @LastEditTime: 2023-03-31 21:48:06
 * @FilePath: \leaflet-display-echarts\src\components\HelloWorld.vue
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
<template>
  <div class="hello">
    <div id="map-dom"></div>
  </div>
</template>

<script lang="ts" setup>
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'
import { onMounted } from 'vue'
import * as echart from 'echarts'

const data = require('../../static/data.json')

onMounted(() => {
  const mapDom = L.map('map-dom').setView([30.75, 120.65], 11)

  L.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 18,
    attribution: 'Map data &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors, ' + '<a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>',
    id: 'osm'
  }).addTo(mapDom)

  data.data.forEach(val => {
    const marker = L.marker(val.location).addTo(mapDom)

    const content = '<div style="width: 220px; height: 220px;" id="marker' + val.id + '"></div>'

    marker.bindPopup(content, {})
    marker.on('popupopen', function (e) {
      const myChart = echart.init(document.getElementById('marker' + val.id)!)

      const option = {
        tooltip: {
          trigger: 'axis'
        },
        xAxis: [{
          type: 'category',
          data: ['1月', '2月', '3月', '4月']
        }],
        yAxis: [{
          type: 'value',
          name: '水量',
          min: 0,
          max: 50,
          interval: 50,
          axisLabel: {
            formatter: '{value} ml'
          }
        }, {
          type: 'value',
          name: '温度',
          min: 0,
          max: 10,
          interval: 5,
          axisLabel: {
            formatter: '{value} °C'
          }
        }],
        series: [{
          name: '蒸发量',
          type: 'bar',
          data: [2.0, 4.9, 7.0, 23.2]
        }, {
          name: '降水量',
          type: 'bar',
          data: [2.6, 5.9, 9.0, 26.4]
        }, {
          name: '平均温度',
          type: 'line',
          yAxisIndex: 1,
          data: [2.0, 2.2, 3.3, 4.5]
        }]
      }

      myChart.setOption(option)
    })
  })
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.hello {
  width: 100%;
  height: 100%;
  #map-dom {
    width: 100%;
    height: 100%;
  }
}

</style>
