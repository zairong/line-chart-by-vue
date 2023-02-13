<template>
  <div class="echart" id="mychart" :style="myChartStyle"></div>
</template>

<script>
import * as echarts from "echarts"
import axios from 'axios'


export default {
  data() {
    return {
      myChart: {},
      xData: ["10S", "20S", "30S", "40S", "50S", "60S", "70S","80S", "90S", "100S", "110S","120S", "130S", "140S", "150S","160S", "170S", "180S"], 
      tokyoData: [], 
      londonData: [], 
      myChartStyle: { float: "left", width: "100%", height: "400px" },
    }
  },
  created() {
    this.fetchData()
    setTimeout(() => {
      clearInterval(this.intervalId)
    }, 600000)
  },
  mounted() {
    this.intervalId = setInterval(this.fetchData, 10000)
 
  },
  methods: {
    initEcharts() {
      const option = {
        xAxis: {
          data: this.xData
        },
        legend: { // 图例
          data: ["tokyo", "london"],
          bottom: "0%"
        },
        yAxis: {},
        series: [
          {
            name: "tokyo",
            data: this.tokyoData,
            type: "line", // 类型设置为折线图
            label: {
              show: true,
              position: "top",
              textStyle: {
                fontSize: 16
              }
            },
            smooth: true
          },
          {
            name: "london",
            data: this.londonData,
            type: "line", // 类型设置为折线图
            label: {
              show: true,
              position: "bottom",
              textStyle: {
                fontSize: 16
              },
              
            },
            smooth: true
          }
        ]
      }
      this.myChart = echarts.init(document.getElementById("mychart"))
      this.myChart.setOption(option)
      //随着屏幕大小调节图表
      window.addEventListener("resize", () => {
        this.myChart.resize()
      })
    },
    async fetchData() {
      const response = await axios.get('http://59.127.3.132:8000/')
      console.log(response.data)
      console.log(response.data.tokyo)
      console.log(response.data.london)
      this.tokyoData.push(response.data.tokyo )
      this.londonData.push(response.data.london)
      this.initEcharts()


    }
  }
}
</script>

