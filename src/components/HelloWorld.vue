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
    const pictures = L.marker(val.location, {
      icon: L.divIcon({
        className: 'leaflet-echart-icon',
        iconSize: [160, 160],
        html: '<div id="marker' + val.id + '" style="width: 160px; height: 160px; position: relative; background-color: transparent;">asd</div>'
      })
    }).addTo(mapDom)

    const myChart = echart.init(document.getElementById('marker' + val.id)!)

    const option = {
      tooltip: {
        trigger: 'item',
        formatter: '{a} <br/>{b}: {c} ({d}%)'
      },
      series: [{
        name: '访问来源',
        type: 'pie',
        radius: ['20', '50'],
        avoidLabelOverlap: false,
        label: {
          normal: {
            show: false,
            position: 'center'
          },
          emphasis: {
            show: true,
            textStyle: {
              fontSize: '18',
              fontWeight: 'bold'
            }
          }
        },
        labelLine: {
          normal: {
            show: false
          }
        },
        data: [{
          value: val.value1,
          name: '直接访问'
        }, {
          value: val.value2,
          name: '邮件营销'
        }, {
          value: val.value3,
          name: '联盟广告'
        }, {
          value: val.value4,
          name: '视频广告'
        }, {
          value: 20,
          name: '搜索引擎'
        }]
      }]
    }

    myChart.setOption(option)
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
