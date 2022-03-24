<template>
  <div class="statistics-main-view">

    <div class="first-screen-center">
      <div class="first-screen-center-info">
        <div class="first-screen-center-left">
          <div class="first-screen-left-days">
            <div class="xmItem">
              <div class="number-info-item"><span class="number-info-title">项目总数(个)：</span><span class="number-info-val">1</span></div>
              <div class="number-info-item"><span class="number-info-title">机器总数(台)：</span><span class="number-info-val">1</span></div>
            </div>
            <div class="xmItem">
              <div class="number-info-item"><span class="number-info-title">测点总数(个)：</span><span class="number-info-val">1</span></div>
              <div class="number-info-item"><span class="number-info-title">运行机器(个)：</span><span class="number-info-val">1</span></div>
            </div>
            <div class="xmItem">
              <div class="number-info-item"><span class="number-info-title">停运机器(个)：</span><span class="number-info-val">1</span></div>
            </div>
          </div>
          <div class="first-screen-left-more">
            <div class="left-more-title">
              警报-健康数据总览
            </div>
            <div class="left-more-state" style="position: relative">
              <div class="echarts-for-react ">
                <PieCharts id="pie-chart2" width="100%" height="280px" :pie-data="pieData2"  style="z-index: 9999" />
                <div class="left-more-state-bg"><div class="left-more-state-bg-tween" style="transform: rotate(62.452deg);" /></div>
              </div>
            </div>
            <div class="state-item-row" style="text-align: center"><div class="state-item"><span class="state-title">健康 </span><span>6台 </span><span>85.71%</span></div><div class="state-item"><span class="state-title">亚健康 </span><span>0台 </span><span>0%</span></div></div>
            <div class="state-item-row " style="text-align: center;margin-top: 15px"><div class="state-item"><span class="state-title">故障 </span><span>1台 </span><span>14.29%</span></div><div class="state-item"><span class="state-title">危险 </span><span>0台 </span><span>0%</span></div></div>
            <div class="state-percentage">报警率：85.71%</div>
            <div style="margin-top: 10px">
              <div class="progressItem">
                <div class="textTitle">警报准确率</div>
                <el-progress :text-inside="true" :stroke-width="22" :percentage="70" />
              </div>
              <div class="progressItem">
                <div class="textTitle">低报准确率</div>
                <el-progress :text-inside="true" :stroke-width="22" :percentage="100" status="success" />
              </div>
              <div class="progressItem">
                <div class="textTitle">高报准确率</div>
                <el-progress :text-inside="true" :stroke-width="22" :percentage="80" status="warning" />
              </div>
              <div class="progressItem">
                <div class="textTitle">高高报准确率</div>
                <el-progress :text-inside="true" :stroke-width="22" :percentage="50" status="exception" />
              </div>
            </div>

          </div>
        </div>
        <div class="first-screen-center-right">
          <div class="center-right-map">
            <mapChart id="map-chart" width="100%" height="670px" :map-data="mapData" />
            <div class="nationwide-info">
              <div class="nationwide-info-title">全国数据总览</div>
              <div class="nationwide-info-border" /><div class="nationwide-info-item"><div class="nationwide-item-title"><div class="nationwide-item-text">正常总数</div></div><div class="nationwide-item-box"><div class="nationwide-item-box-percentage" style="width: 0%;" /></div><div class="nationwide-item-box-val">0</div></div><div class="nationwide-info-item"><div class="nationwide-item-title"><div class="nationwide-item-text">异常</div></div><div class="nationwide-item-box"><div class="nationwide-item-box-percentage" style="width: 1%;" /></div><div class="nationwide-item-box-val">1</div></div><div class="nationwide-info-item"><div class="nationwide-item-title"><div class="nationwide-item-text">未激活</div></div><div class="nationwide-item-box"><div class="nationwide-item-box-percentage" style="width: 0%;" /></div><div class="nationwide-item-box-val">0</div></div>
            </div>
            <div class="first-screen-header">
              <div class="header-date">{{ localeTime }}</div>
            </div>
          </div>
          <div class="center-right-charts">
            <el-row :gutter="10" class="mt20">
              <el-col :xs="24" :sm="12" :md="8" :lg="8" :xl="8"><div class="partBox">
                <!--                        <span class="lineStep" />各厂商应用数量分析-->
                <GradientBar id="infeed-bar4" :chart-data="barData4" :x-axis-data="xAxisData4" :is-show-legend="true" width="100%" height="300px" />
              </div></el-col>
              <el-col :xs="24" :sm="12" :md="8" :lg="8" :xl="8"><div class="partBox">
                <!--                        <span class="lineStep" />各厂商应用使用次数分析-->
                <LineCharts id="line-chart2" width="100%" height="300px" :chart-data="lineData2" :x-axis-data="xAxisDataLline2" :is-show-legend="true" />
              </div></el-col>
              <el-col :xs="24" :sm="12" :md="8" :lg="8" :xl="8"><div class="partBox partIndex">
                <!--                        <span class="lineStep" />应用类别统计-->
                <el-select v-model="value" placeholder="请选择">
                  <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value"
                  />
                </el-select>
                <LineChartsBase id="line-chart" width="100%" height="300px" :chart-data="lineData" :x-axis-data="xAxisDataLline" :is-show-legend="false" />
              </div></el-col>
            </el-row>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import LineCharts from '@/components/Charts/LineMarker'
import mapChart from '@/components/Charts/mapEcharts'
import PieCharts from '@/components/Charts/PieEcharts'
import GradientBar from '@/components/Charts/GradientBar'
import LineChartsBase from '@/components/Charts/LineCharts'
export default {
  name: 'Dashboard',
  components: { PieCharts, mapChart, LineCharts, GradientBar, LineChartsBase },
  data() {
    return {
      barData: [],
      yAxisData: [],
      titleText: '',
      colorArr: [],
      pieData: [],
      pieData2: [],
      lineData: [],
      xAxisDataLline: [],
      colorArr2: [],
      lineData2: [],
      xAxisDataLline2: [],
      mapData: [],
      barData3: [],
      xAxisData3: [],
      barData4: [],
      xAxisData4: [],
      mapQuery: {
        yyLb: '1'
      },
      options: [{
        value: '选项1',
        label: '潍坊港BM1机器 / 电机自由端1H'
      }, {
        value: '选项2',
        label: '双皮奶'
      }, {
        value: '选项3',
        label: '蚵仔煎'
      }, {
        value: '选项4',
        label: '龙须面'
      }, {
        value: '选项5',
        label: '北京烤鸭'
      }],
      value: '潍坊港BM1机器 / 电机自由端1H',
      setTime: '',
      localeTime: ''
    }
  },
  created() {
  },
  mounted() {
    this.initData()
    this.setTime = setInterval(this.getDate, 1000)
  },
  // 离开当前页面后执行
  destroyed: function() {
    window.clearInterval(this.setTime)
  },
  methods: {
    getDate() {
      var date = new Date()
      var date1 = date.toLocaleString()
      // var div1 = document.getElementById('times')
      // div1.innerHTML = date1
      const dateStr = date1.split(' ')
      const yearStr = dateStr[0].split('/')
      const nowDte = yearStr[0] + '年' + yearStr[1] + '月' + yearStr[2] + '日'
      var str = '星期' + '日一二三四五六'.charAt(new Date().getDay())
      this.localeTime = str + ' ' + nowDte + ' ' + dateStr[1]
    },
    initData() {
      this.getLineyy()
      this.getMapData()
      this.getBarCs2()
      this.pieYylb()
      this.getLineBase()
    },
    getLineyy() {
      this.lineData2 = [
        {
          name: '低报',
          data: [
            {
              name: '2020-02-23',
              value: '0'
            },
            {
              name: '2020-02-24',
              value: '0'
            },
            {
              name: '2020-02-25',
              value: '0'
            },
            {
              name: '2020-02-26',
              value: '1'
            },
            {
              name: '2020-02-27',
              value: '0'
            }, {
              name: '2020-02-28',
              value: '0'
            },
            {
              name: '2020-03-01',
              value: '5'
            }
          ]
        },
        {
          name: '高报',
          data: [
            {
              name: '2020-02-23',
              value: '0'
            },
            {
              name: '2020-02-24',
              value: '0'
            },
            {
              name: '2020-02-25',
              value: '0'
            },
            {
              name: '2020-02-26',
              value: '0'
            },
            {
              name: '2020-02-27',
              value: '1'
            }, {
              name: '2020-02-28',
              value: '0'
            },
            {
              name: '2020-03-01',
              value: '0'
            }
          ]
        },
        {
          name: '高高报',
          data: [
            {
              name: '2020-02-23',
              value: '0'
            },
            {
              name: '2020-02-24',
              value: '0'
            },
            {
              name: '2020-02-25',
              value: '0'
            },
            {
              name: '2020-02-26',
              value: '0'
            },
            {
              name: '2020-02-27',
              value: '0'
            }, {
              name: '2020-02-28',
              value: '0'
            },
            {
              name: '2020-03-01',
              value: '0'
            }
          ]
        },

      ]
      for (let p = 0; p < this.lineData2[0].data.length; p++) {
        this.xAxisDataLline2.push(this.lineData2[0].data[p].name)
      }
    },
    randomData() {
      return Math.round(Math.random() * 500)
    },
    getMapData() {
      // var bj=[
      //   {
      //     name:'报警',
      //     value:'10'
      //   },{
      //   name:'未报警',
      //     value: '3'
      //   },{
      //   name:'未激活',
      //     value:'5'
      //   }
      // ]
      var bj = ['40', '10', '5']
      var bj1 = ['400', '100', '500']
      var bj2 = ['300', '500', '600']
      var bj3 = ['200', '100', '50']

      this.mapData = [{ name: '北京', value: bj },
        { name: '天津', value: bj },
        { name: '上海', value: bj1 },
        { name: '重庆', value: bj1 },
        { name: '河北', value: bj1 },
        { name: '河南', value: bj },
        { name: '云南', value: bj },
        { name: '辽宁', value: bj },
        { name: '黑龙江', value: bj },
        { name: '湖南', value: bj2 },
        { name: '安徽', value: bj2 },
        { name: '山东', value: bj },
        { name: '新疆', value: bj },
        { name: '江苏', value: bj2 },
        { name: '浙江', value: bj },
        { name: '江西', value: bj },
        { name: '湖北', value: bj },
        { name: '广西', value: bj },
        { name: '甘肃', value: bj },
        { name: '山西', value: bj3 },
        { name: '内蒙古', value: bj3 },
        { name: '陕西', value: bj3 },
        { name: '吉林', value: bj },
        { name: '福建', value: bj },
        { name: '贵州', value: bj },
        { name: '广东', value: bj },
        { name: '青海', value: bj },
        { name: '西藏', value: bj },
        { name: '四川', value: bj },
        { name: '宁夏', value: bj },
        { name: '海南', value: bj },
        { name: '台湾', value: bj },
        { name: '香港', value: bj3 },
        { name: '澳门', value: bj }]
      // this.mapData = [{ name: '济南', value: 998 },
      //   { name: '青岛', value: 745 },
      //   { name: '淄博', value: 631 },
      //   { name: '烟台', value: 422 },
      //   { name: '潍坊', value: 782 }]
    },
    getBarCs2() {
      this.barData4 = [
        {
          name: '启用设备',
          data: [
            {
              name: '2020-02-23',
              value: '17'
            },
            {
              name: '2020-02-24',
              value: '5'
            },
            {
              name: '2020-02-25',
              value: '0'
            },
            {
              name: '2020-02-26',
              value: '7'
            },
            {
              name: '2020-02-27',
              value: '3'
            }, {
              name: '2020-02-28',
              value: '5'
            },
            {
              name: '2020-03-01',
              value: '10'
            }
          ]
        },
        {
          name: '停机设备',
          data: [
            {
              name: '2020-02-23',
              value: '20'
            },
            {
              name: '2020-02-24',
              value: '2'
            },
            {
              name: '2020-02-25',
              value: '4'
            },
            {
              name: '2020-02-26',
              value: '0'
            },
            {
              name: '2020-02-27',
              value: '8'
            }, {
              name: '2020-02-28',
              value: '5'
            },
            {
              name: '2020-03-01',
              value: '3'
            }
          ]
        }
      ]
      for (let p = 0; p < this.barData4[0].data.length; p++) {
        this.xAxisData4.push(this.barData4[0].data[p].name)
      }
    },
    pieYylb() {
      this.pieData2 = [{ name: '故障', value: 11 },
        { name: '危险', value: 7 },
        { name: '亚健康', value: 6 },
        { name: '健康', value: 2 },
      ]
    },
    getLineBase() {
      // var data = []
      // var now = +new Date()
      // var value = Math.random() * 1000
      // for (var i = 0; i < 60; i++) {
      //   now = new Date(+now + this.oneDay)
      //   data.push(this.randomData2(now, value))
      // }
      // this.lineData = [{
      //   name: '1',
      //   value: ['1', '1']
      // }, {
      //   name: '2',
      //   value: ['2', '20']
      // }]
      // var randomData = [{
      //   name: '3',
      //   value: ['3', '10']
      // }, {
      //   name: '4',
      //   value: ['4', '60']
      // }]
      //
      // setInterval(function() {
      //   for (var i = 0; i < 5; i++) {
      //     this.lineData.shift()
      //     // this.lineData.push(randomData)
      //     this.lineData.concat(randomData)
      //   }
      // })
    },
    // randomData2(now, value) {
    //   value = Math.random() * 100
    //   var valueName =
    //     now.getFullYear() +
    //     '/' +
    //     (now.getMonth() + 1) +
    //     '/' +
    //     now.getDate() +
    //     ' ' +
    //     (now.getHours() >= 10 ? now.getHours() : '0' + now.getHours()) +
    //     ':' +
    //     (now.getMinutes() >= 10 ? now.getMinutes() : '0' + now.getMinutes()) +
    //     ':' +
    //     (now.getSeconds() >= 10 ? now.getSeconds() : '0' + now.getSeconds())
    //   return {
    //     name: now.toString(),
    //     value: [valueName, Math.round(value)],
    //   }
    // },
  }
}
</script>
<style lang="scss" scoped>

</style>
<style>
    .partIndex .el-input--medium .el-input__inner{
        height: 20px;
        line-height: 20px;
        background-color: #0F355A;
        margin-left: 70px;
        width: 240px;
    }
    .partIndex .el-select .el-input .el-select__caret{
        display: none;
    }
</style>
