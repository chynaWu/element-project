<template>
  <div class="statistics-main-view" style="height: 100%;background: none">

    <div class="first-screen-center" style="height: 100%">
      <div class="first-screen-center-info" style="height: 100%">
        <div class="leftBox">
          <div class="titleBox"><span class="lineStep" />数据选择</div>
          <div class="leftTree">
            <TreeInfo ref="bortherTree" @handleClick="handleClick" @saveDwObj="saveDwObj" />
          </div>
          <div v-if="type=='1'" class="leftTable">
            <el-table
              v-if="tableData.length>0"
              :data="tableData2"
              style="width: 100%"
              height="250"
            >
              <el-table-column label="" width="40" fixed>
                <template scope="scope">
                  <el-radio
                    v-model="currentRow"
                    :label="scope.$index"
                    style="color: #fff; padding-left: 10px; margin-right: -25px"
                    @change.native="getCurrentRow(scope.row)"
                  />
                </template>
              </el-table-column>
              <el-table-column
                fixed
                prop="createTime"
                label="采集时间"
              />
              <el-table-column
                fixed
                label="有效值"
              >
                <template slot-scope="scope"> <span>{{ fun(scope.row.temp) }}</span></template>
              </el-table-column>
            </el-table>
            <el-pagination
              v-if="total>0"
              class="renderPage"
              :current-page="pageOptions.currentPage"
              :page-sizes="[10, 20, 30, 40]"
              :page-size="pageOptions.pageSize"
              layout="total, sizes, prev, pager, next"
              :total="total"
              @size-change="changeSizeHandle"
              @current-change="currentChangeHandle"
            />
            <div v-if="tableData.length==0">
              暂无数据
            </div>
          </div>
        </div>
        <div class="rightBox">
          <div class="btnList">
            <el-button :class="{'activeBtn':isA}" type="primary" plain @click="part1">趋势分析</el-button>
            <el-button :class="{'activeBtn':isB}" type="primary" plain @click="part2">时域分析</el-button>
            <el-button :class="{'activeBtn':isC}" type="primary" plain @click="part3">频谱分析</el-button>
            <el-button :class="{'activeBtn':isD}" type="primary" plain @click="part4">温度趋势</el-button>
          </div>
          <div class="chartsMain">
            <div v-if="isA" class="partBox">
              <div v-if="lineData.length>0" style="width: 100%;">
                <LineCharts id="line-chart" width="100%" height="300px" :type="type" :chart-data="lineData" :x-axis-data="xAxisDataLline" :is-show-legend="false" @clickLine="clickLine" />
                <div style="margin-bottom: 15px">测点路径：{{ treeObj.pppidLabel }} \ {{ treeObj.ppidLabel }} \ {{ treeObj.pidLabel }} \ {{ treeObj.label }} \ 采样时间：{{ cjDate }} \ 游标：<span class="subText">{{ lineToTipObj.xLabel }} \ {{ fun(lineToTipObj.yValue) }}</span></div>
              </div>
              <div v-if="lineData.length==0" class="nodata">
                暂无数据
              </div>
            </div>
            <div v-if="isB" class="partBox">
              <LineCharts v-if="lineData2.length>0" id="line-chart2" width="100%" height="300px" :type="type2" :chart-data="lineData2" :x-axis-data="xAxisDataLline2" :is-show-legend="false" @clickLine2="clickLine2" />
              <div v-if="lineData2.length>0" style="margin-bottom: 15px">测点路径：{{ treeObj.pppidLabel }} \ {{ treeObj.ppidLabel }}\ {{ treeObj.ppidLabel2 }} \ {{ treeObj.pidLabel }} \ {{ treeObj.label }}\ 采样时间：{{ cjDate }} \ 游标：<span class="subText">{{ lineToTipObj2.xLabel }} \ {{ fun(lineToTipObj2.yValue) }}</span></div>
              <div v-if="lineData2.length==0" class="nodata">
                暂无数据
              </div>
            </div>
            <div v-if="isC" class="partBox">
              <LineCharts v-if="lineData3.length>0" id="line-chart3" width="100%" height="300px" :type="type3" :chart-data="lineData3" :x-axis-data="xAxisDataLline3" :is-show-legend="false" @clickLine3="clickLine3" />
              <div v-if="lineData3.length>0" style="margin-bottom: 15px">测点路径：{{ treeObj.pppidLabel }} \ {{ treeObj.ppidLabel }} \ {{ treeObj.pidLabel }} \ {{ treeObj.label }} \ 采样时间：{{ cjDate }} \ 游标：<span class="subText">{{ lineToTipObj3.xLabel }} \ {{ lineToTipObj3.yValue }}</span></div>
              <div v-if="lineData3.length==0" class="nodata">
                暂无数据
              </div>
            </div>
            <div v-if="isD" class="partBox">
              <LineCharts v-if="lineData4.length>0" id="line-chart4" width="100%" height="300px" :type="type4" :chart-data="lineData4" :x-axis-data="xAxisDataLline4" :is-show-legend="false" @clickLine4="clickLine4" />
              <div v-if="lineData4.length>0" style="margin-bottom: 15px">测点路径：{{ treeObj.pppidLabel }} \ {{ treeObj.ppidLabel }} \ {{ treeObj.pidLabel }} \ {{ treeObj.label }} \ 采样时间：{{ cjDate }} \ 游标：<span class="subText">{{ lineToTipObj4.xLabel }} \ {{ lineToTipObj4.yValue }}</span></div>
              <div v-if="lineData4.length==0" class="nodata">
                暂无数据
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div></template>

<script>
import TreeInfo from '@/views/yygl/components/TreeInfo'
import LineCharts from '@/components/Charts/LineChartstp'
export default {
  name: 'Dashboard',
  components: {
    TreeInfo, LineCharts
  },
  data() {
    return {
      expandAll: true,
      tableData: [],
      tableData2: [],
      type: '',
      typeValue: '',
      defaultProps: {
        children: 'children',
        label: 'Name'
      },
      defaultExpandedids: [],
      typeItemObj: {
        labelName: '皮带机',
        type: 'U'
      },
      total: 0,
      pageOptions: {
        currentPage: 1, // 当前页
        pageSize: 10, // 展示页数
      },
      currentRow: '',
      lineData: [],
      xAxisDataLline: [],
      lineData2: [],
      xAxisDataLline2: [],
      lineData3: [],
      xAxisDataLline3: [],
      lineData4: [],
      xAxisDataLline4: [],
      activeNames: '1',
      treeObj: {
        pppidLabel: '',
        ppidLabel: '',
        ppidLabel2: '',
        pidLabel: '',
        label: ''
      },
      cjDate: '',
      lineToTipObj: {
        xLabel: '',
        yValue: ''
      },
      lineToTipObj2: {
        xLabel: '',
        yValue: ''
      },
      lineToTipObj3: {
        xLabel: '',
        yValue: ''
      },
      lineToTipObj4: {
        xLabel: '',
        yValue: ''
      },
      type: '1',
      type2: '2',
      type3: '3',
      type4: '4',
      isA: true,
      isB: false,
      isC: false,
      isD: false,
      configMap: {},
      treeCilckId: '',
      treeClickType: '',
    }
  },
  watch: {
    // lineToTipObj: {
    //   handler(val) {
    //     console.log('深度监听：', val)
    //     this.lineToTipObj.xLabel = val.xLabel
    //     this.lineToTipObj.yValue = val.yValue
    //   },
    //   deep: true,
    //   immediate: true
    // }
  },
  created() {
    if (JSON.parse(sessionStorage.getItem('firstObj')) !== null) {
      const data = JSON.parse(sessionStorage.getItem('firstObj'))
      this.treeObj.ppidLabel = data.ppidLabel
      this.treeObj.pidLabel = data.pidLabel
      this.treeObj.ppidLabel2 = data.pidLevel2
      this.treeObj.label = data.label
      // this.initGet()
    }
    this.configMap = { timeStart: '2021-05-08 16:30:00', timeEnd: new Date() }
    // this.initGet()
  },
  mounted() {

  },
  methods: {
    fun(val) {
      return Number(val).toFixed(2)
    },
    initGet() {
      // this.getTable()
      if (this.isA == true) {
        this.getCharts()
      }
      if (this.isB == true) {
        this.getCharts2()
      } if (this.isC == true) {
        this.getCharts3()
      } if (this.isD == true) {
        this.getCharts4()
      }
      // this.getCharts2()
      // this.getCharts3()
      // this.getCharts4()
    },
    part1() {
      this.isA = !this.isA
      this.$nextTick(function() {
        this.getCharts()// 通过axios拿到的后台数据填充的echarts图表方法
      })
    },
    part2() {
      this.isB = !this.isB
      this.$nextTick(function() {
        this.getCharts2()// 通过axios拿到的后台数据填充的echarts图表方法
      })
    },
    part3() {
      this.isC = !this.isC
      this.$nextTick(function() {
        this.getCharts3()// 通过axios拿到的后台数据填充的echarts图表方法
      })
    },
    part4() {
      this.isD = !this.isD
      this.$nextTick(function() {
        this.getCharts4()// 通过axios拿到的后台数据填充的echarts图表方法
      })
    },
    clickLine(data) {
      const obj = JSON.parse(data)
      this.lineToTipObj.xLabel = obj.xLabel
      this.lineToTipObj.yValue = obj.yValue
      // console.log('66', this.lineToTipObj)
    },
    clickLine2(data) {
      const obj = JSON.parse(data)
      this.lineToTipObj2.xLabel = obj.xLabel
      this.lineToTipObj2.yValue = obj.yValue
      // console.log('66', this.lineToTipObj)
    },
    clickLine3(data) {
      const obj = JSON.parse(data)
      this.lineToTipObj3.xLabel = obj.xLabel
      this.lineToTipObj3.yValue = obj.yValue
      // console.log('66', this.lineToTipObj3)
    },
    clickLine4(data) {
      const obj = JSON.parse(data)
      this.lineToTipObj4.xLabel = obj.xLabel
      this.lineToTipObj4.yValue = obj.yValue
      // console.log('66', this.lineToTipObj3)
    },
    handleChange(val) {
      console.log('333333333', val)
      if (val.name == '1') {
        this.$nextTick(function() {
          this.getCharts()// 通过axios拿到的后台数据填充的echarts图表方法
        })
      } else if (val.name == '2') {
        this.$nextTick(function() {
          this.getCharts2()// 通过axios拿到的后台数据填充的echarts图表方法
        })
      } else if (val.name == '3') {
        this.$nextTick(function() {
          this.getCharts3()
        })
      } else if (val.name == '4') {
        this.$nextTick(function() {
          this.getCharts4()
        })
      }
    },
    handleClick(data) {
      console.log(data)
      this.treeObj.ppidLabel = data.ppid
      this.treeObj.pidLabel = data.pid
      this.treeObj.ppidLabel2 = data.pidLevel2
      this.treeObj.label = data.label
      const dataType = data.Number.split('_')[1]
      // if (data.type == undefined) {
      //   this.treeCilckId = data.value.split('_')[0]
      //   if (dataType == 'A') {
      //     this.treeClickType = '1'
      //   }
      //   if (dataType == 'S') {
      //     this.treeClickType = '2'
      //   }
      //   this.initGet()
      // }
      this.initGet()
    },
    saveDwObj(data) {
      this.treeObj.pppidLabel = data
    },

    getCurrentRow(row) {
      console.log('11', row)
      this.cjDate = row.createTime
      console.log('cjDate', this.cjDate)
    },
    // 每页显示的条数改变
    changeSizeHandle(pageSize) {
      this.pageOptions.pageSize = pageSize
      this.getList2()
    },

    // current-change用于监听页数改变，而内容也发生改变
    currentChangeHandle(page) {
      this.pageOptions.currentPage = page
      this.getList2()
    },
    getList2() {
      this.tableData2 = this.tableData.slice((this.pageOptions.currentPage - 1) * this.pageOptions.pageSize, (this.pageOptions.currentPage) * this.pageOptions.pageSize)
    },
    getCharts() {
      const params = {
        algorithmId: this.treeClickType,
        createTime: this.cjDate,
        equipmentId: this.treeCilckId,
        type: '',
        configMap: this.configMap
      }
      this.$api.analysis(params).then(response => {
        if (response.code === '0') {
          this.tableData = response.data
          this.total = response.data.length
          this.tableData2 = this.tableData.slice(0, this.pageOptions.pageSize) // 初始化一下展示数据集
          // this.lineData = response.data
          const dataArr = response.data
          const jsonArr = JSON.parse(JSON.stringify(dataArr).replace(/createTime/g, 'name').replace(/effectiveValueV/g, 'value'))
          const arr = [
            {
              name: '',
              data: jsonArr
            }
          ]
          this.lineData = arr
          for (let p = 0; p < this.lineData[0].data.length; p++) {
            this.xAxisDataLline.push(this.lineData[0].data[p].name)
          }
        } else {
          this.$message.error(response.message)
        }
      })
      // this.lineData = [
      //   {
      //     name: 'pinpu',
      //     data: [
      //       {
      //         name: '2020-02-23',
      //         value: '1'
      //       },
      //       {
      //         name: '2020-02-24',
      //         value: '1'
      //       },
      //       {
      //         name: '2020-02-25',
      //         value: '1'
      //       },
      //       {
      //         name: '2020-02-26',
      //         value: '1'
      //       },
      //       {
      //         name: '2020-02-27',
      //         value: '0'
      //       }, {
      //         name: '2020-02-28',
      //         value: '0'
      //       },
      //       {
      //         name: '2020-03-01',
      //         value: '5'
      //       }
      //     ]
      //   },
      // ]
      // for (let p = 0; p < this.lineData[0].data.length; p++) {
      //   this.xAxisDataLline.push(this.lineData[0].data[p].name)
      // }
    },
    getCharts2() {
      const params = {
        algorithmId: 24,
        createTime: this.cjDate,
        equipmentId: this.treeCilckId,
        type: this.treeClickType,
        configMap: { timeStart: '', timeEnd: '' }
      }
      this.$api.analysis(params).then(response => {
        if (response.code === '0') {
          this.tableData = response.data
          this.total = response.data.length
          this.tableData2 = this.tableData.slice(0, this.pageOptions.pageSize)
          // this.lineData2 = response.data
          const dataArr = response.data
          const jsonArr = JSON.parse(JSON.stringify(dataArr).replace(/createTime/g, 'name').replace(/effectiveValueV/g, 'value'))
          const arr = [
            {
              name: '',
              data: jsonArr
            }
          ]
          this.lineData2 = arr
          for (let p = 0; p < this.lineData2[0].data.length; p++) {
            this.xAxisDataLline2.push(this.lineData2[0].data[p].name)
          }
        } else {
          this.$message.error(response.message)
        }
      })
      // this.lineData2 = [
      //   {
      //     name: 'pinpu',
      //     data: [
      //       {
      //         name: '2020-02-23',
      //         value: '1'
      //       },
      //       {
      //         name: '2020-02-24',
      //         value: '1'
      //       },
      //       {
      //         name: '2020-02-25',
      //         value: '1'
      //       },
      //       {
      //         name: '2020-02-26',
      //         value: '1'
      //       },
      //       {
      //         name: '2020-02-27',
      //         value: '0'
      //       }, {
      //         name: '2020-02-28',
      //         value: '0'
      //       },
      //       {
      //         name: '2020-03-01',
      //         value: '5'
      //       }
      //     ]
      //   },
      // ]
      // for (let p = 0; p < this.lineData2[0].data.length; p++) {
      //   this.xAxisDataLline2.push(this.lineData2[0].data[p].name)
      // }
    },
    getCharts3() {
      const params = {
        algorithmId: 7,
        createTime: this.cjDate,
        equipmentId: this.treeCilckId,
        type: this.treeClickType,
        configMap: {}
      }
      this.$api.analysis(params).then(response => {
        if (response.code === '0') {
          // this.lineData3 = response.data
          this.tableData = response.data
          this.total = response.data.length
          this.tableData2 = this.tableData.slice(0, this.pageOptions.pageSize)
          const dataArr = response.data
          const jsonArr = JSON.parse(JSON.stringify(dataArr).replace(/createTime/g, 'name').replace(/effectiveValueV/g, 'value'))
          const arr = [
            {
              name: '',
              data: jsonArr
            }
          ]
          this.lineData3 = arr
          for (let p = 0; p < this.lineData3[0].data.length; p++) {
            this.xAxisDataLline3.push(this.lineData3[0].data[p].name)
          }
        } else {
          this.$message.error(response.message)
        }
      })
      // this.lineData3 = [
      //   {
      //     name: 'pinpu',
      //     data: [
      //       {
      //         name: '2020-02-23',
      //         value: '1'
      //       },
      //       {
      //         name: '2020-02-24',
      //         value: '1'
      //       },
      //       {
      //         name: '2020-02-25',
      //         value: '1'
      //       },
      //       {
      //         name: '2020-02-26',
      //         value: '1'
      //       },
      //       {
      //         name: '2020-02-27',
      //         value: '0'
      //       }, {
      //         name: '2020-02-28',
      //         value: '0'
      //       },
      //       {
      //         name: '2020-03-01',
      //         value: '5'
      //       }
      //     ]
      //   },
      // ]
      // for (let p = 0; p < this.lineData3[0].data.length; p++) {
      //   this.xAxisDataLline3.push(this.lineData3[0].data[p].name)
      // }
    },
    getCharts4() {
      const params = {
        algorithmId: 5,
        createTime: this.cjDate,
        equipmentId: this.treeCilckId,
        type: this.treeClickType,
        configMap: this.configMap
      }
      this.$api.analysis(params).then(response => {
        if (response.code === '0') {
          this.tableData = response.data
          this.total = response.data.length
          this.tableData2 = this.tableData.slice(0, this.pageOptions.pageSize)
          // this.lineData4 = response.data
          const dataArr = response.data
          const jsonArr = JSON.parse(JSON.stringify(dataArr).replace(/createTime/g, 'name').replace(/effectiveValueV/g, 'value'))
          const arr = [
            {
              name: '',
              data: jsonArr
            }
          ]
          this.lineData4 = arr
          for (let p = 0; p < this.lineData4[0].data.length; p++) {
            this.xAxisDataLline4.push(this.lineData4[0].data[p].name)
          }
        } else {
          this.$message.error(response.message)
        }
      })
    }
  }
}
</script>
<style lang="scss" scoped>
    .activeBtn{
        background-color: #409eff;
        border-color: #409eff;
        color: #fff;
    }
    .btnList{
        margin: 15px 0;
    }
    .partBox{
        flex: 1;
        box-shadow: 0 1px 4px rgba(0, 0, 0, .15);
        /*border-color: rgba(0, 0, 0, .05);*/
        /*height: 320px;*/
        overflow: hidden;
        margin-right: 10px;
        position: relative;
        margin-bottom: 15px;
    }
    .first-screen-center{
        box-shadow: none;
    }
    .subText{
        color: #3A71A8;
    }
    .first-screen-center-info{
        /*justify-content: center;*/
        /*align-items: center;*/
        width: 100%;
        text-align: center;
        margin: auto;
        overflow: hidden;
        padding: 20px;
    }
    .titleBox{
        position: relative;
        padding-left: 16px;
        text-align: left;
        font-size: 17px;
        font-weight: 700;
        line-height: 40px;
        border-bottom: 1px solid #E9EEF7;
    }
    .lineStep{
        width: 3px;
        height: 16px;
        background-color: #5e6268;
        display: inline-block;
        margin-right: 5px;
        vertical-align: middle;
        margin-top: -3px;
    }
    .leftBox{
        width: 300px;
        height: 100%;
        padding: 15px;
        max-height: 100%;
        overflow-y: auto;
        box-sizing: border-box;
        padding-right: 10px;
        background: #fff;
        border-right: 10px solid #f2f2f2;
        /*display: flex;*/
        /*flex-direction: column;*/
        .leftTree{
            /*overflow: auto;*/
            /*flex: 1;*/
            height: calc(100% - 430px);
            overflow-y: auto;
        }
        .leftTable{
            /*height: 450px;*/
        }
    }
    .rightBox{
        flex: 1;
        max-height: 100%;
        overflow-y: auto;
        background: #fff;
    }
    .infoImg{
        width:260px ;
        margin: 0 auto;
        margin-bottom: 15px;
        /*height:260px ;*/
        img{
            width: 100%;
        }
    }
    .nodata{
        height: 300px;
        display: flex;
        justify-content: center;
        align-items: center;
    }
</style>
<style>
    .renderPage .el-pager{
        display: inline-block;
    }
    .renderPage .btn-prev,.renderPage .el-pagination__jump{
        display: block !important;
    }
    .renderPage .el-pager li{
        min-width: 26px;
    }
    .el-table .cell{
        line-height: 20px;
    }
    .el-table--medium th, .el-table--medium td{
        padding: 5px 0;
    }
    .el-collapse-item__header{
        background-color: #f5f7fa;
        border-bottom: 1px solid #dfe4ed;
        margin: 0;
        padding: 0 15px;
        font-size: 18px;
    }
    .el-input--medium .el-input__inner{
        background-color:#fff !important;
        width: 30px;
    }
</style>
