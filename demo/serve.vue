<template>
  <div class="container">
    <button @click="clickBtn">更新数据</button>
    <div style="width: 400px; height: 300px;">
      <bar-chart
        :title="title"
        :data="axisData"
        :series="[{
            name: '',
            type: 'bar',
            label: {
                show: true,
                position: 'top'
            },
            data: valueData,
            itemStyle: {
              normal: {
                color: '#917DF7'
              }
            },
            barWidth: '15px'
          }]"
      />
    </div>

    <button @click="secondClickBtn">更新数据2</button>
    <div style="width: 400px; height: 300px;">
      <axis-bar-chart
        :title="title"
        :data="secondAxisData"
        :series="[{
            name: '',
            type: 'bar',
            label: {
                show: true,
                position: 'right'
            },
            data: secondValueData,
            itemStyle: {
              normal: {
                color: '#917DF7'
              }
            },
            barWidth: '15px'
          }]"
      />
    </div>

    <button @click="secondClickBtn">折线图 LineChart</button>
    <div style="width: 400px; height: 300px;">
      <line-chart
        :title="title"
        :data="secondAxisData"
        :series="[{
            name: '',
            type: 'line',
            stack: '金额',
            data: this.secondValueData,
            symbol: 'circle', //折线点设置为实心点
            symbolSize: 8, //折线点的大小
            itemStyle: {
              normal: {
                color: '#41d9ed', //折线点的颜色
                lineStyle: {
                  width: 1.5,
                  color: '#41d9ed' //折线的颜色
                }
              }
            }
          }]"
      />
    </div>

    <button @click="pieClickBtn">饼图 PieChart</button>
    <div style="width: 600px; height: 400px;">
      <pie-chart
        :title="title"
        :data="secondAxisData"
        :series="[{
          name: '',
          type: 'pie',
          radius: '55%',
          center: ['50%', '50%'],
          label: {
            alignTo: 'edge',
            formatter: '{d}%\n{c}亿元',
            minMargin: 5,
            edgeDistance: 10,
            lineHeight: 15,
            rich: {
              time: {
                fontSize: 10,
                color: '#999'
              }
            }
          },
          data: this.threeValueData
        }]"
      />
    </div>
    

    <!-- <button @click="gismapClickBtn">地图 GisMap</button>
    <div style="width: 600px; height: 400px;">
      <gis-map
        :type="'advance'"
        :mapArea="'shandong'"
        :areaData="[]"
        :lineData="[]"
        :mapJson="[]"
      />
    </div> -->

  </div>
</template>

<script>
import {BarChart, AxisBarChart, LineChart, PieChart, GisMap} from '@/index'

export default {
  components: { BarChart, AxisBarChart, LineChart, PieChart, GisMap },
  data() {
    return {
      title: "demo",
      axisData: ['1'],
      valueData: [222],
      secondAxisData: ['1', '2'],
      secondValueData: [333, 33],
      threeValueData: [{value: 1048, name: '搜索引擎'},
        {value: 735, name: '直接访问'},
        {value: 580, name: '邮件营销'},
        {value: 484, name: '联盟'},
        {value: 484, name: '联盟1'},
        {value: 484, name: '联盟2'},
        {value: 300, name: '视频广告'}
      ]
    }
  },
  methods: { 
    clickBtn: function () {
      let num = Math.floor(Math.random() * 10 + 1)
      let tArr = []
      let vArr = []
      for(let i = 0;i<num;i++) {
        tArr.push(i)
        vArr.push(Math.floor(Math.random() * 100 + 1))
      }

      this.axisData = tArr
      this.valueData = vArr
    },
    secondClickBtn: function () {
      let num = Math.floor(Math.random() * 10 + 1)
      let tArr = []
      let vArr = []
      for(let i = 0;i<num;i++) {
        tArr.push(i)
        vArr.push(Math.floor(Math.random() * 100 + 1))
      }

      this.secondAxisData = tArr
      this.secondValueData = vArr
    },
    pieClickBtn: function () {
      let num = Math.floor(Math.random() * 10 + 1)
      let vArr = []
      for(let i = 0;i<num;i++) {
        vArr.push({
          name: i,
          value: Math.floor(Math.random() * 100 + 1)
        })
      }

      this.threeValueData = vArr
    }
  },
  created() { },
}
</script>