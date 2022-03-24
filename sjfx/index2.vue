<template>
  <div class="dashboard-container mlr200" style="min-height: calc(100vh - 273px)">
    <el-row class="el-nav" :gutter="10">
      <el-col :xs="24" :sm="12" :md="12" :lg="12" :xl="12"><div class="navPart" @click="jumpYYgl">
        <img src="@/icons/img/left2.png">
      </div></el-col>
      <el-col :xs="24" :sm="12" :md="12" :lg="12" :xl="12"><div class="navPart navPart2" @click="jumpQxgl">
        <img src="@/icons/img/right2.png">
      </div></el-col>
    </el-row>
    <el-row class="subTitle">
        <el-button style="float: right;margin-top: -8px" type="primary" class="searchBtn" @click="jumpShyh">审核用户信息</el-button>
        <img src="@/icons/img/iconRight.png" class="mr5">应用统计分析
      <el-form ref="listQuery" :model="listQuery" :inline="true" style="float: right;margin-top: -8px">
        <el-form-item label="选择日期:" prop="xzDate">
          <el-date-picker
            v-model="listQuery.xzDate"
            type="daterange"
            range-separator="至"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
            style="width: 380px"
            :picker-options="pickerOptions0"
            @change="selectDate"
          />
        </el-form-item>
      </el-form>
    </el-row>
    <el-row :gutter="10" class="mt30">
      <el-col :xs="24" :sm="12" :md="8" :lg="8" :xl="8"><div class="partBox">
        <span class="lineStep" />应用使用量排行
        <barInfeed id="infeed-bar2" :chart-data="barData2" :y-axis-data="yAxisData2" :is-show-legend="false" :color-arr="colorArr2" width="100%" height="450px" />
      </div></el-col>
      <el-col :xs="24" :sm="12" :md="8" :lg="8" :xl="8"><div class="partBox" style="position: relative">
        <span class="lineStep" />应用使用情况总览
        <el-select v-model="mapQuery.yyLb" placeholder="请选择" value-key="value" style="width: 120px" class="selectBox2" @change="changeForm($event)">
          <el-option label="案件审批" value="1" />
          <el-option label="事项审批" value="2" />
          <el-option label="事项申请" value="3" />
          <el-option label="在办案件" value="4" />
        </el-select>
        <mapChart id="map-chart" width="100%" height="430px" :map-data="mapData" />
      </div></el-col>
      <el-col :xs="24" :sm="12" :md="8" :lg="8" :xl="8"><div class="partBox">
        <span class="lineStep" />应用使用次数趋势分析
        <LineCharts id="line-chart2" width="100%" height="450px" :chart-data="lineData2" :x-axis-data="xAxisDataLline2" :is-show-legend="true" />
      </div></el-col>
    </el-row>
    <el-row :gutter="10" class="mt10">
      <el-col :xs="24" :sm="12" :md="8" :lg="8" :xl="8"><div class="partBox">
        <span class="lineStep" />各厂商应用数量分析
        <barCharts id="infeed-bar3" :chart-data="barData3" :x-axis-data="xAxisData3" :is-show-legend="false" width="100%" height="450px" />
      </div></el-col>
      <el-col :xs="24" :sm="12" :md="8" :lg="8" :xl="8"><div class="partBox">
        <span class="lineStep" />各厂商应用使用次数分析
        <GradientBar id="infeed-bar4" :chart-data="barData4" :x-axis-data="xAxisData4" :is-show-legend="true" width="100%" height="450px" />
      </div></el-col>
      <el-col :xs="24" :sm="12" :md="8" :lg="8" :xl="8"><div class="partBox">
        <span class="lineStep" />应用类别统计
        <PieCharts id="pie-chart2" width="100%" height="460px" :pie-data="pieData2" :radius="radius" />
      </div></el-col>
    </el-row>
  </div>

</template>

<script>
import barInfeed from '@/components/Charts/barInfeedEcharts'
import PieECharts from '@/components/Charts/PieCharts'
import LineCharts from '@/components/Charts/LineMarker'
import mapChart from '@/components/Charts/mapEcharts'
import PieCharts from '@/components/Charts/PieEcharts'
import barCharts from '@/components/Charts/barEcharts'
import GradientBar from '@/components/Charts/GradientBar'
import LineMarker from '@/components/Charts/LineMarkerPoint'
export default {
  name: 'Dashboard',
  components: { barInfeed, PieECharts, LineCharts, mapChart, PieCharts, barCharts, GradientBar, LineMarker },
  data() {
    return {
      listQuery: {
        kssj: '',
        jssj: ''
      },
      barData: [],
      yAxisData: [],
      titleText: '',
      colorArr: [],
      pieData: [],
      pieData2: [],
      radius: ['40%', '50%'],
      pickerOptions0: {
        disabledDate(time) {
          return time.getTime() > Date.now()
        }
      },
      lineData: [],
      xAxisDataLline: [],
      barData2: [],
      yAxisData2: [],
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
      }
    }
  },
  created() {
  },
  mounted() {
    this.initData()
  },
  methods: {
    changeForm(e) {
      this.$forceUpdate()
    },
    selectDate() {

    },
    jumpYYgl() {
      this.$router.push({ path: '/yygl' })
    },
    jumpQxgl() {
      this.$router.push({ path: '/qxgl' })
    },
    jumpShyh() {
      this.$router.push({ path: '/shyh' })
    },
    initData() {
      this.getyysyl()
      this.getLineyy()
      this.getMapData()
      this.getBarCs()
      this.getBarCs2()
      this.pieYylb()
    },
    getyysyl() {
      // let dataList
      const dataList = [{ name: '案件审批', value: 20 },
        { name: '事项审批', value: 15 },
        { name: '事项申请', value: 10 },
        { name: '在办案件', value: 10 }]
      this.barData2 = [
        {
          name: '使用量',
          data: dataList.reverse()
        }
      ]
      for (let p = 0; p < this.barData2[0].data.length; p++) {
        this.yAxisData2.push(this.barData2[0].data[p].name)
      }
      this.colorArr2 = [{ top: '#83D7C5', bottom: 'rgba(131,215,197, 0.1)' }]
    },
    getLineyy() {
      this.lineData2 = [
        {
          name: '案件审批',
          data: [
            {
              name: '2020-01',
              value: '17'
            },
            {
              name: '2020-02',
              value: '23'
            },
            {
              name: '2020-03',
              value: '15'
            },
            {
              name: '2020-04',
              value: '13'
            },
            {
              name: '2020-05',
              value: '13'
            },
            {
              name: '2020-06',
              value: '10'
            },
            {
              name: '2020-07',
              value: '20'
            }, {
              name: '2020-08',
              value: '8'
            },
            {
              name: '2020-09',
              value: '12'
            },
            {
              name: '2020-10',
              value: '11'
            }
          ]
        },
        {
          name: '事项审批',
          data: [
            {
              name: '12-01',
              value: '15'
            },
            {
              name: '12-02',
              value: '17'
            },
            {
              name: '12-03',
              value: '10'
            },
            {
              name: '12-04',
              value: '22'
            },
            {
              name: '12-05',
              value: '12'
            },
            {
              name: '12-06',
              value: '5'
            },
            {
              name: '12-07',
              value: '7'
            }, {
              name: '12-08',
              value: '16'
            },
            {
              name: '12-09',
              value: '12'
            },
            {
              name: '12-10',
              value: '21'
            }
          ]
        },
        {
          name: '事项申请',
          data: [
            {
              name: '12-01',
              value: '10'
            },
            {
              name: '12-02',
              value: '7'
            },
            {
              name: '12-03',
              value: '3'
            },
            {
              name: '12-04',
              value: '5'
            },
            {
              name: '12-05',
              value: '6'
            },
            {
              name: '12-06',
              value: '7'
            },
            {
              name: '12-07',
              value: '4'
            }, {
              name: '12-08',
              value: '9'
            },
            {
              name: '12-09',
              value: '12'
            },
            {
              name: '12-10',
              value: '6'
            }
          ]
        },
        {
          name: '在办案件',
          data: [
            {
              name: '12-01',
              value: '32'
            },
            {
              name: '12-02',
              value: '27'
            },
            {
              name: '12-03',
              value: '30'
            },
            {
              name: '12-04',
              value: '26'
            },
            {
              name: '12-05',
              value: '28'
            },
            {
              name: '12-06',
              value: '27'
            },
            {
              name: '12-07',
              value: '29'
            }, {
              name: '12-08',
              value: '31'
            },
            {
              name: '12-09',
              value: '33'
            },
            {
              name: '12-10',
              value: '35'
            }
          ]
        }
      ]
      for (let p = 0; p < this.lineData2[0].data.length; p++) {
        this.xAxisDataLline2.push(this.lineData2[0].data[p].name)
      }
    },
    getMapData() {
      this.mapData = [{ code: '370000', flcount: null, flname: null, name: '山东省', sl: 11728, value: null },
        { code: '370100', flcount: null, flname: null, name: '济南市', sl: 100, value: 998 },
        { code: '370200', flcount: null, flname: null, name: '青岛市', sl: 40, value: 40 },
        { code: '370300', flcount: null, flname: null, name: '淄博市', sl: 0, value: 631 },
        { code: '370400', flcount: null, flname: null, name: '枣庄市', sl: 0, value: null },
        { code: '370500', flcount: null, flname: null, name: '东营市', sl: 0, value: null },
        { code: '370600', flcount: null, flname: null, name: '烟台市', sl: 0, value: 422 },
        { code: '370700', flcount: null, flname: null, name: '潍坊市', sl: 0, value: 782 },
        { code: '370800', flcount: null, flname: null, name: '济宁市', sl: 0, value: null },
        { code: '370900', flcount: null, flname: null, name: '泰安市', sl: 0, value: null },
        { code: '371000', flcount: null, flname: null, name: '威海市', sl: 0, value: null },
        { code: '371100', flcount: null, flname: null, name: '日照市', sl: 0, value: null },
        { code: '371300', flcount: null, flname: null, name: '临沂市', sl: 0, value: null },
        { code: '371400', flcount: null, flname: null, name: '德州市', sl: 0, value: null },
        { code: '371500', flcount: null, flname: null, name: '聊城市', sl: 0, value: null },
        { code: '371600', flcount: null, flname: null, name: '滨州市', sl: 0, value: null },
        { code: '371700', flcount: null, flname: null, name: '菏泽市', sl: 0, value: null }]
      // this.mapData = [{ name: '济南', value: 998 },
      //   { name: '青岛', value: 745 },
      //   { name: '淄博', value: 631 },
      //   { name: '烟台', value: 422 },
      //   { name: '潍坊', value: 782 }]
    },
    getBarCs() {
      this.barData3 = [
        {
          name: '应用量',
          data: [
            {
              name: '北明',
              value: '14'
            },
            {
              name: '同方赛威迅',
              value: '2'
            },
            {
              name: '华宇',
              value: '0'
            }
          ]
        }
      ]
      for (let p = 0; p < this.barData3[0].data.length; p++) {
        this.xAxisData3.push(this.barData3[0].data[p].name)
      }
    },
    getBarCs2() {
      this.barData4 = [
        {
          name: '案件审批',
          data: [
            {
              name: '北明',
              value: '17'
            },
            {
              name: '同方赛威迅',
              value: '5'
            },
            {
              name: '华宇',
              value: '0'
            }
          ]
        },
        {
          name: '事项申请',
          data: [
            {
              name: '北明',
              value: '20'
            },
            {
              name: '同方赛威迅',
              value: '2'
            },
            {
              name: '华宇',
              value: '4'
            }
          ]
        },
        {
          name: '事项审批',
          data: [
            {
              name: '北明',
              value: '23'
            },
            {
              name: '同方赛威迅',
              value: '7'
            },
            // {
            //   name: '华宇',
            //   value: '1'
            // }
          ]
        },
        {
          name: '在办案件',
          data: [
            {
              name: '北明',
              value: '21'
            },
            // {
            //   name: '同方赛威迅',
            //   value: '3'
            // },
            // {
            //   name: '华宇',
            //   value: '3'
            // }
          ]
        },
        // {
        //   name: '风险评估',
        //   data: [
        //     {
        //       name: '同方赛威迅',
        //       value: '21'
        //     },
        //
        //   ]
        // },
      ]
      for (let p = 0; p < this.barData4[0].data.length; p++) {
        this.xAxisData4.push(this.barData4[0].data[p].name)
      }
      // this.xAxisData4 = ['北明', '同方赛威迅', '华宇']
    },
    pieYylb() {
      this.pieData2 = [{ name: '公共', value: 11 },
        { name: '案管', value: 7 },
        { name: '公益诉讼', value: 6 },
        { name: '未检', value: 2 },
        { name: '控申', value: 2 }]
    }
  }
}
</script>
<style lang="scss" scoped>
    .el-nav{
        margin-top: 20px;
        padding: 10px;
        margin-bottom: 30px;
        background-color: #fff;
        .navPart{
            cursor: pointer;
            height: 130px;
            background: url('../../icons/img/left1.png')no-repeat;
            background-size: 100% 100%;
            img{
                margin-left: 115px;
                margin-top:35px;
            }
        }
        .navPart2{
            background: url('../../icons/img/right1.png')no-repeat;
        }
    }
    .filter-index{
        margin: 0 auto;
        text-align: center;
    }

</style>
<style>
    .el-form-item__label,.el-form-item__content{
        font-size: 16px;
    }
    .filter-index .el-input--medium .el-input__inner{
        height: 55px;
        line-height: 55px;
        padding-right: 0;
    }
    .filter-index .searchBtn{
        height: 52px;
        line-height: 52px;
        padding: 0 20px;
        background-color: #3d6fd7;
        border-radius: 6px;
        margin-left: 10px;
    }
    .filter-index .filter-item .el-input__inner{
        width: 690px;
        margin-left: -7px;
        border-radius: 0 4px 4px 0;
    }
    .filter-index .el-form--inline .el-form-item{
        margin-right: 0;
    }
    .el-form--inline .el-form-item{
        margin-bottom: 0;
    }
    .selectBox2{
        position: absolute;
        right: 10px;
        top: 25px;
        z-index: 1000;
    }
</style>
