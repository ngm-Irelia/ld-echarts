<template>
  <div
    ref="gisMapBox"
    style="width: 100%; height: 100%"
  ></div>
</template>

<script>
import * as echarts from 'echarts'
/**
 * 地图
 * 地图的使用必须传入 mapJson， 地图轮廓的经纬度信息
 */
export default {
  name: 'GisMap',
  props: {
    type: {
      type: String,
      default: 'advance', // base // advance 
    },
    mapArea: {
      type: String,
      default: 'shandong'
    },
    areaData: {
      type: Array,
      default: () => {
        return []
      },
    },
    lineData: {
      type: Array,
      default: () => {
        return []
      },
    },
    mapJson: {
      type: Array,
      default: () => {
        return []
      },
    }
  },
  data() {
    return {
      myChart: '',
    }
  },
  mounted() {
    let box = this.$refs.gisMapBox
    this.addGisMap(
      box,
      this.$props.type,
      this.$props.mapArea,
      this.$props.areaData,
      this.$props.lineData,
    )
  },
  watch: {
    areaData(newData, oldData) {
      if(!this.diff(newData, oldData)) {
        let box = this.$refs.gisMapBox
        this.addGisMap(
          box,
          this.$props.type,
          this.$props.mapArea,
          this.$props.areaData,
          this.$props.lineData,
        )
      }
    },
    lineData(newData, oldData) {
      if(!this.diff(newData, oldData)) {
        let box = this.$refs.gisMapBox
        this.addGisMap(
          box,
          this.$props.type,
          this.$props.mapArea,
          this.$props.areaData,
          this.$props.lineData,
        )
      }
    },
  },
  methods: {
    /**
     * 地图
     * @param {*} dom 图表存放的元素
     * @param {*} type 地图显示方式 // base：基本的，只有显示地图功能 // advance：高级geo方式
     * @param {*} mapArea string 显示的地图范围 // shandong china
     * @param {*} areaData 地图上每个区域的数据
     * @param {*} linesData 线条数据
     */
    addGisMap(dom, type, mapArea, areaData, linesData) {
      console.log(type)
      console.log(mapArea)
      console.log(areaData)
      console.log(linesData)
      /* if (this.myChart) {
        this.myChart.dispose()
      } */
      this.myChart = echarts.init(dom)


      let CityEnum = {
        '东营市': [118.625299, 37.636119],
        '临沂市': [118.286436, 35.311894],
        '威海市': [122.000809, 37.118689],
        '德州市': [116.653994, 37.251363],
        '日照市': [119.146499, 35.578656],
        '枣庄市': [117.39817, 34.916234],
        '泰安市': [117.030947, 36.002333],
        '济南市': [117.221244, 36.639974],
        '济宁市': [116.74105, 35.371092],
        '淄博市': [118.058672, 36.610968],
        '滨州市': [117.847396, 37.542717],
        '潍坊市': [119.077723, 36.554349],
        '烟台市': [120.805129, 37.241857],
        '聊城市': [115.887733, 36.460089],
        '菏泽市': [115.698013, 35.152536],
        '青岛市': [120.150851, 36.451234],
      }

      let CityList = ['东营市', '临沂市', '威海市', '德州市', '日照市', '枣庄市', '泰安市', '济南市', '济宁市', '淄博市', '滨州市', '潍坊市', '烟台市', '聊城市', '菏泽市', '青岛市'];
      
      let maxNum = 0;
      areaData.forEach((item) => {
        if(item.value > maxNum) {
          maxNum = item.value
        }
      })

      // 点数据
      let newAreaData = []
      for(let i=0;i<CityList.length;i++){
        let cityItem = '';
        for(let j=0;j<areaData.length;j++){
          if(CityList[i] == areaData[j].name) {
            cityItem = areaData[j]
          }
        }
        if(cityItem) {
          let newItem = JSON.parse(JSON.stringify(cityItem));
          newItem.num = newItem.value;
          if(CityEnum.hasOwnProperty(cityItem.name)) {
            newItem.value = [...CityEnum[cityItem.name], newItem.value]
          }
          newAreaData.push(newItem)
        }else{
          newAreaData.push({
            name: CityList[i],
            value: CityEnum[CityList[i]],
            num: ''
          })
        }
      }

      // 线数据
      let lineData = []
      linesData.forEach((item) => {
        if(CityEnum.hasOwnProperty(item.city)) {
          lineData.push({
            fromName: '济南市', // 起点城市name
            toName: item.city, // 终点城市name
            coords: [
              [117.221244, 36.639974], // 起点城市坐标
              CityEnum[item.city] // 终点城市坐标
            ],
          })
        }
      })

      this.myChart.registerMap('shandong', mapJson);

      let option = {

        visualMap: {
          min: 0,
          max: maxNum,
          show: false,
          text: ['High', 'Low'],
          realtime: false,
          calculable: true,
          inRange: {
            color: ['#4D7BA9', '#386EA8', '#2860A7', '#1C4B91', '#123A7B']
          }
        },
        geo: {
          geoIndex: 1,
          zoom: 1.1,
          map: 'shandong',
          label: {
            position: 'top',
            distance: 150,
            formatter: '{b}\n{c}',
            show: false,
            color: '#fff',
            normal: {
                show: false,
                formatter: '{b}\n{c}',
                textStyle: {
                  formatter: '{b}\n{c}',
                    color: '#fff'
                }
            },
            emphasis: {
              show: false,
              color: '#fff',
              formatter: '{b}\n{c}',
            }
          },
          itemStyle: {
            normal: {
            color: '#fff',
            areaColor: '#4E545F',
            borderColor: '#87929F',
            borderWidth: 1,
            
            }
          },
          data: newAreaData,
        },
        series: [
          {
            name: 'mapLines',
            type: 'lines',
            zlevel: 1,
            symbolSize: 6,
            effect: {
              show: true,
              period: 4, // 箭头指向速度，值越小速度越快
              trailLength: 0.2, // 特效尾迹的长度。取从 0 到 1 的值，数值越大尾迹越长。
              symbol: 'arrow', // 特效图形的标记。
              symbolSize: 6 // 特效标记的大小
            },
            lineStyle: {
              normal: {
                width: 1, // 尾迹线条宽度
                color: '#1DE9B6', // 线颜色 4ab2e5 1DE9B6   1DE9B6 FF4500
                opacity: 0.1, // 尾迹线条透明度
                curveness: 0.2 // 边的曲度，支持从 0 到 1 的值，值越大曲度越大。
              }
            },
            data: lineData
          },
          {
            type: 'scatter', //类型，表示点数据
            coordinateSystem: 'geo',
            data: newAreaData, //数据源
            symbolSize: 1, //标记点大小
            //标记样式：可使用svg图标
            //symbol: 'path://M877.632 456.8c14.976-14.72 20.384-32.96 14.816-49.984-5.536-17.024-20.608-28.544-41.344-31.584l-190.24-27.84c-6.976-1.024-18.464-9.472-21.6-15.904l-85.12-173.696c-9.28-18.944-24.896-29.76-42.88-29.76-17.952 0-33.6 10.816-42.816 29.76l-85.12 173.696c-3.104 6.432-14.592 14.848-21.6 15.904l-190.24 27.84c-20.704 3.04-35.776 14.56-41.344 31.584-5.568 17.024-0.16 35.232 14.816 49.984l137.696 135.232c5.088 4.992 9.536 18.816 8.32 25.92l-32.48 190.912c-3.552 20.832 2.752 38.816 17.344 49.344 7.52 5.44 16.224 8.16 25.472 8.16 8.576 0 17.6-2.336 26.56-7.04l170.176-90.176c6.048-3.2 20.448-3.2 26.528 0l170.144 90.112c18.528 9.856 37.504 9.44 52.064-1.056 14.56-10.528 20.864-28.48 17.344-49.28l-32.48-190.976c-1.28-7.104 3.2-20.928 8.32-25.92l137.664-135.232z',
            //角度信息
            symbolRotate: 0,
            //鼠标选中说明
            label: {
              distance: 50,
              normal: {
                  formatter: function (item) {
                    return `${item.name}\n${item.data.num}`
                  },
                  show: true, //是否一直显示
                  color: '#fff',
                  fontWeight: '400'
              },
              emphasis: {
                  show: true,
                  distance: 50,
                  color: '#fff',
              }
            },
          },
          {
            geoIndex: 0,
            name: 'yh',
            type: 'map',
            mapType: 'shandong',
            zoom: 1.2,
            data: newAreaData,
            symbolSize: function(val) {
              return val[2] / 15;
            },
            label: {
              position: 'top',
              distance: 50,
              formatter: '{b}\n{c}',
              show: false,
              color: '#fff',
              emphasis: {
                show: false,
                color: '#fff'
              },
              normal: {
                  formatter: '{b}\n{c}', //tooltip显示，自定义参考上方例子
                  position: 'left', //对齐
                  show: false //是否一直显示
              }
            },
            itemStyle: {
              normal: {
              color: '#fff',
              areaColor: '#4E545F',
              borderColor: '#87929F',
              borderWidth: 1
              }
            },
            zlevel: 13
          },
        ]

      };
      this.myChart.setOption(option);
      window.addEventListener("resize", function() {
        this.myChart.resize();
      });
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
