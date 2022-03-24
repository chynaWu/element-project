<template>
  <div class="statistics-main-view" style="height: 100%;background: none">

    <div class="first-screen-center" style="box-shadow: none">
      <div
        class="first-screen-center-info"
        style="position: relative;"
      >
        <!--        <gaudeChartsC id="gaudeChart2" :chart-data="gaudeData2" width="100%" height="300px" :colorGau="colorGau"/>-->
        <div class="firstBox">
          <div class="titleBox"><span class="lineStep" />警报信息</div>
          <el-row :gutter="10" class="mt10" style=" margin-left: 10px;">
            <el-col :xs="24" :sm="12" :md="17" :lg="17" :xl="17" class="partFirst">
              <div class="partBox" style="position: relative">
                <gaudeChartsC id="gaudeChart" :chart-data="gaudeData" width="100%" height="300px" :color-gau="colorGau" />
                <div class="itemName">警报事件准确率</div>
                <div class="itemName itemName2">（近30天）</div>
              </div>
              <div class="partBox" style="position: relative">
                <gaudeChartsC id="gaudeChart2" :chart-data="gaudeData2" width="100%" height="300px" :color-gau="colorGau2" />
                <div class="itemName">低报事件准确率</div>
                <div class="itemName itemName2">（近30天）</div>
              </div>
              <div class="partBox" style="position: relative">
                <gaudeChartsC id="gaudeChart3" :chart-data="gaudeData3" width="100%" height="300px" :color-gau="colorGau3" />
                <div class="itemName">高报事件准确率</div>
                <div class="itemName itemName2">（近30天）</div>
              </div>
              <div class="partBox" style="position: relative;margin-right: 0px">
                <gaudeChartsC id="gaudeChart4" :chart-data="gaudeData4" width="100%" height="300px" :color-gau="colorGau4" />
                <div class="itemName">高高报事件准确率</div>
                <div class="itemName itemName2">（近30天）</div>
              </div>
            </el-col>
            <el-col :xs="24" :sm="12" :md="7" :lg="7" :xl="7" class="partFirst">
              <div class="partBox" style="position: relative">
                  <el-row :gutter="12">
                      <el-col :span="24">
                          <el-card shadow="always" class="cardBody">
                              <img src="../../assets/jb1.png" height="30" width="30"/> <el-tooltip class="item" effect="dark" content="MAP智能守护机器(台)：7" placement="top"><span> MAP智能守护机器(台)：7</span>
                              </el-tooltip>
                             </el-card>

                      </el-col>
                      <el-col :span="24">
                          <el-card shadow="always" class="cardBody">
                              <img src="../../assets/jb2.png" height="30" width="30"/>
                              <el-tooltip class="item" effect="dark" content="单台每天平均(个)：0.05" placement="top"><span>单台每天平均(个)：0.05</span></el-tooltip>
                          </el-card>
                      </el-col>
                      <el-col :span="24">
                          <el-card shadow="always" class="cardBody">
                              <img src="../../assets/Jb3.png" height="30" width="30"/>
                              <el-tooltip class="item" effect="dark" content="机器100天累计警报事件(个)：34" placement="top"><span>机器100天累计警报事件(个)：34</span></el-tooltip>
                          </el-card>
                      </el-col>
                      <el-col :span="24">
                          <el-card shadow="always" class="cardBody">
                              <img src="../../assets/jb4.png" height="30" width="30"/>
                              <el-tooltip class="item" effect="dark" content="每天平均警报数(个)：0.34" placement="top"><span>每天平均警报数(个)：0.34</span></el-tooltip>
                          </el-card>
                      </el-col>
                  </el-row>
              </div>
            </el-col>
          </el-row>
        </div>
        <div class="firstBox" style="margin-top: 20px">
          <el-row :gutter="10" class="mt10" style=" margin-left: 10px;">
            <el-col :xs="24" :sm="12" :md="16" :lg="16" :xl="16" class="partFirst">
              <div class="partBox" style="position: relative;height: 460px">
                <span class="lineStep" /><span class="titleSub">近期报警总数趋势</span>
                <LineCharts id="line-chart2" width="100%" height="400px" :chart-data="lineData2" :x-axis-data="xAxisDataLline2" :is-show-legend="true" :colorArr="colorArr" :splitLine="splitLine"/>
              </div>
            </el-col>
            <el-col :xs="24" :sm="12" :md="8" :lg="8" :xl="8" class="partFirst"><div class="partBox" style="height: 460px">
              <span class="lineStep" /><span class="titleSub">高报/高高报7天累计总数TOP5</span>
<!--              <barEcharts id="infeed-bar4" :chart-data="barData4" :x-axis-data="xAxisData3"  :is-show-legend="true" width="100%" height="400px" />-->
                <div >  <barCharts  id="infeed-bar3" :chart-data="barData3" :x-axis-data="xAxisData3" :is-show-legend="false" width="100%" height="400px" :colorArr="colorArr2"/></div>
                <div v-if="barData3.length==0" class="noNum">您最近7天暂无数据</div>
            </div></el-col>
          </el-row>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import gaudeChartsC from '@/components/Charts/gaudeChartsC'
import LineCharts from '@/components/Charts/LineMarker'
import barCharts from '@/components/Charts/barEcharts'
export default {
  name: 'Dashboard',
  components: { gaudeChartsC, LineCharts, barCharts },
  data() {
    return {
      gaudeData: [],
      gaudeData2: [],
      gaudeData3: [],
      gaudeData4: [],
      colorGau: '#67c23a',
      colorGau3: '#e6a23c',
      colorGau4: '#f56c6c',
      colorGau2: '#FFD800',
      lineData2: [],
      xAxisDataLline2: [],
      barData3: [],
      xAxisData3: [],
      colorArr:[{ top: '#FFD800', bottom: 'rgba(255, 216, 0, 0.1)' }, { top: '#e6a23c', bottom: 'rgba(230, 162, 60, 0.1)' }, { top: '#f56c6c', bottom: 'rgba(245, 108, 108, 0.1)' }, { top: '#ffbe3f', bottom: 'rgba(255, 190, 63, 0.1)' }, { top: '#4185ef', bottom: 'rgba(65, 133, 239, 0.1)' }, { top: '#38d7b7', bottom: 'rgba(56, 215, 183, 0.1)' }],
      splitLine:'#ddd',
      colorArr2:[{ top: '#6C91C4', bottom: 'rgba(108, 145, 196, 0.1)' }]
    }
  },
  created() {
    this.gaudeData = [
      { value: 70 },
    ]
    this.gaudeData3 = [
      { value: 70 },
    ]
    this.gaudeData2 = [
      { value: 70 },
    ]
    this.gaudeData4 = [
      { value: 70 },
    ]

  },
  mounted() {
    this.getLineyy()
    this.getBarCs()
  },

  methods: {
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
              value: '1'
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
    getBarCs() {
      this.barData3 = [
        {
          name: '应用量',
          data: [
            {
              name: '潍焦管1号',
              value: '14'
            },
            {
              name: '潍焦管2号',
              value: '2'
            }
          ]
        }
      ]
      for (let p = 0; p < this.barData3[0].data.length; p++) {
        this.xAxisData3.push(this.barData3[0].data[p].name)
      }
    },

  }
}
</script>
<style lang="scss" scoped>

    .lineStep{
        width: 3px;
        height: 16px;
        background-color: #5e6268;
        display: inline-block;
        margin-right: 5px;
        vertical-align: middle;
        margin-top: -3px;
    }
  .nationwide-info{
      top:20px;
      right: 20px;
  }
    .nationwide-item-text{
        font-size: 20px;
    }
  .nationwide-item-title,.nationwide-item-box-val{
      height: 36px;
      line-height: 36px;
  }
    .nationwide-item-title{
        width: 120px;
    }
    .nationwide-item-box-val{
        font-size: 22px;
    }
    .first-screen-center-info{
        flex-direction: column;
    }
    .titleSub{
        font-size: 17px;
        line-height: 40px;
    }
    .firstBox{
        width: 100%;
        background-color: #fff;
        padding: 15px 0;
        .titleBox{
            position: relative;
            padding-left: 16px;
            text-align: left;
            font-size: 17px;
            font-weight: 700;
            line-height: 40px;
        }
    }
    .partFirst{
       display: flex;
        flex-direction: row;
        /*margin-right: 10px;*/

    }
    .partBox{
        flex: 1;
        box-shadow: 0 1px 4px rgba(0, 0, 0, .15);
        /*border-color: rgba(0, 0, 0, .05);*/
        height: 320px;
        overflow: hidden;
        margin-right: 10px;
        position: relative;
        /*border-right: 10px solid #fff;*/
    }
    .itemName{
        font-size: 16px;
        line-height: 30px;
        color: #938989;
        position: absolute;
        left: 30% ;
        bottom: 60px;
    }
    .itemName2{
        bottom: 30px;
    }
    .noNum{
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 20px;
        margin-top: 160px;
    }
</style>
<style>
    .cardBody .el-card__body{
        padding: 15px !important;
    background-color: rgba(243, 243, 243, 0.6);
        margin-top: 15px;
    }
    .cardBody .el-card__body img{
        vertical-align: middle;
        margin-top: -4px;
        width: 24px;
        height: 24px;
        margin-right: 10px;
    }
    .el-input--medium .el-input__inner{
        height: 20px;
        line-height: 20px;
        background-color: #0F355A;
        margin-left: 70px;
        width: 240px;
    }
    .el-select .el-input .el-select__caret{
        display: none;
    }

</style>
