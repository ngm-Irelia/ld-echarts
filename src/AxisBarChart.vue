<template>
  <div
    ref="axisBarChartBox"
    style="width: 100%; height: 100%"
  ></div>
</template>

<script>
import * as echarts from 'echarts'

/**
 * 横向柱状图
 */
export default {
  name: 'AxisBarChart',
  props: {
    id: {
      type: String,
      default: '',
    },
    title: {
      type: String,
      default: '',
    },
    data: {
      type: Array,
      default: () => {
        return []
      },
    },
    series: {
      type: Array,
      default: () => {
        return []
      },
    },
  },
  data() {
    return {
      myChart: '',
    }
  },
  mounted() {
    let box = this.$refs.axisBarChartBox
    this.addAxisBarChart(
      this.$props.title,
      box,
      this.$props.data,
      this.$props.series,
    )
  },
  watch: {
    series(newSeries, oldSeries) {
      if(!this.diff(newSeries, oldSeries)) {
        let box = this.$refs.axisBarChartBox
        this.addAxisBarChart(
          this.$props.title,
          box,
          this.$props.data,
          this.$props.series,
        )
      }
    },
  },
  methods: {
    /**
     * 柱状图
     * @param {*} title 标题
     * @param {*} dom 图表存放的元素
     * @param {*} data 数据
     * @param {*} series 配置
     */
    addAxisBarChart(title, dom, data, series) {
      if (this.myChart) {
        this.myChart.dispose()
      }
      this.myChart = echarts.init(dom)
      this.myChart.setOption({
        title: {
          text: title,
        },
        tooltip: {},
        grid: {
          left: '3%',
          right: '4%',
          bottom: '3%',
          top: '5%',
          containLabel: true,
        },
        xAxis: {
          type: 'value',
          boundaryGap: [0, 0.01],
          //网格样式
          splitLine: {
            show: true,
            lineStyle: {
              color: ['#adf4fd'],
              width: 0.3,
              type: 'solid'
            }
          },
          axisTick: {
            show: false
          },
          axisLabel: {
            show: true,
            textStyle: {
              color: '#fff'
            }
          },
        },
        yAxis: {
          type: 'category',
          data: data,
          axisLabel: {
            show: true,
            textStyle: {
              color: '#fff'
            }
          },
        },
        series: series,
      })
    },
    diff(obj1, obj2) {
      var o1 = obj1 instanceof Object
      var o2 = obj2 instanceof Object
      if (!o1 || !o2) {
        /*  判断不是对象  */
        console.log(obj1)
        console.log(obj2)
        return obj1 === obj2
      }

      if (Object.keys(obj1).length !== Object.keys(obj2).length) {
        return false
        //Object.keys() 返回一个由对象的自身可枚举属性(key值)组成的数组,例如：数组返回下表：let arr = ["a", "b", "c"];console.log(Object.keys(arr))->0,1,2;
      }

      for (var attr in obj1) {
        var t1 = obj1[attr] instanceof Object
        var t2 = obj2[attr] instanceof Object
        if (t1 && t2) {
          if (!this.diff(obj1[attr], obj2[attr])) {
            return false
          }
        } else if (obj1[attr] !== obj2[attr]) {
          return false
        }
      }
      return true
    },
  },
}
</script>
