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
      <bar-chart
        :title="title"
        :data="secondAxisData"
        :series="[{
            name: '',
            type: 'bar',
            label: {
                show: true,
                position: 'top'
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
  </div>
</template>

<script>
import BarChart from '@/BarChart'

export default {
  components: { BarChart },
  data() {
    return {
      title: "demo",
      axisData: ['1'],
      valueData: [222],
      secondAxisData: ['1', '2'],
      secondValueData: [333, 33],
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
    }
  },
  created() { },
}
</script>