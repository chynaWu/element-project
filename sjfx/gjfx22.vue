<template>
  <div class="statistics-main-view" style="height: 100%;background: none">

    <div class="first-screen-center" style="height: 100%">
      <div class="first-screen-center-info" style="height: 100%">
        <div class="leftBox">
          <div class="titleBox"><span class="lineStep" />数据选择</div>
          <div class="leftTree">
            <TreeInfo ref="bortherTree" @handleClick="handleClick" @saveDwObj="saveDwObj" />
          </div>
          <div class="leftTable">
            <el-table
              :data="tableData"
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
                prop="date"
                label="采集时间"
              />
              <el-table-column
                fixed
                prop="date"
                label="有效值"
              />
            </el-table>
            <el-pagination
              class="renderPage"
              :current-page="currentPage"
              :page-sizes="[10, 20, 30, 40]"
              :page-size="10"
              layout="total, sizes, prev, pager, next, jumper"
              :total="total"
              @size-change="changeSizeHandle"
              @current-change="currentChangeHandle"
            />
          </div>
        </div>
        <div class="rightBox">
          <el-collapse v-model="activeNames" accordion @change="handleChange">
            <el-collapse-item title="趋势分析" name="1">
              <LineCharts id="line-chart" width="100%" height="300px" :type="type" :chart-data="lineData" :x-axis-data="xAxisDataLline" :is-show-legend="false" @clickLine="clickLine" />
              <div>测点路径：{{ treeObj.pppidLabel }} \ {{ treeObj.ppidLabel }} \ {{ treeObj.pidLabel }} \ {{ treeObj.label }} \ 采样时间：{{ cjDate }} \ 游标：<span class="subText">{{ lineToTipObj.xLabel }} \ {{ lineToTipObj.yValue }}</span></div>
            </el-collapse-item>
            <el-collapse-item title="时域分析" name="2">
              <LineCharts id="line-chart2" width="100%" height="300px" :type="type2" :chart-data="lineData2" :x-axis-data="xAxisDataLline2" :is-show-legend="false" @clickLine2="clickLine2" />
              <div>测点路径：{{ treeObj.pppidLabel }} \ {{ treeObj.ppidLabel }} \ {{ treeObj.pidLabel }} \ {{ treeObj.label }}\ 采样时间：{{ cjDate }} \ 游标：<span class="subText">{{ lineToTipObj2.xLabel }} \ {{ lineToTipObj2.yValue }}</span></div>
            </el-collapse-item>
            <el-collapse-item title="频谱分析" name="3">
              <LineCharts id="line-chart3" width="100%" height="300px" :type="type3" :chart-data="lineData3" :x-axis-data="xAxisDataLline3" :is-show-legend="false" @clickLine3="clickLine3" />
              <div>测点路径：{{ treeObj.pppidLabel }} \ {{ treeObj.ppidLabel }} \ {{ treeObj.pidLabel }} \ {{ treeObj.label }} \ 采样时间：{{ cjDate }} \ 游标：<span class="subText">{{ lineToTipObj3.xLabel }} \ {{ lineToTipObj3.yValue }}</span></div>
            </el-collapse-item>
          </el-collapse>
        </div>
      </div>
    </div>
  </div>

</template>

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
      total: 100,
      currentPage: 1,
      size: 10,
      currentRow: '',
      lineData: [],
      xAxisDataLline: [],
      lineData2: [],
      xAxisDataLline2: [],
      lineData3: [],
      xAxisDataLline3: [],
      activeNames: ['1'],
      treeObj: {
        pppidLabel: '',
        ppidLabel: '',
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
      type: '1',
      type2: '2',
      type3: '3'
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
      this.treeObj.label = data.label
    }
    this.tableData = [
      {
        date: '2016-05-02',
        name: '王小虎',
        index: '1',
        address: '上海市普陀区金沙江路 1518 弄',
      },
      {
        date: '2016-05-04',
        name: '王小虎',
        index: '2',
        address: '上海市普陀区金沙江路 1517 弄',
      },
      {
        date: '2016-05-01',
        name: '王小虎',
        index: '3',
        address: '上海市普陀区金沙江路 1519 弄',
      },
      {
        date: '2016-05-03',
        name: '王小虎',
        index: '3',
        address: '上海市普陀区金沙江路 1516 弄',
      },
    ]
  },
  mounted() {
    this.getCharts()
    this.getCharts2()
    this.getCharts3()
  },
  methods: {
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
    handleChange(val) {
      // console.log(val)
      if (val == '1') {
        this.$nextTick(function() {
          this.getCharts()// 通过axios拿到的后台数据填充的echarts图表方法
        })
      } else if (val == '2') {
        this.$nextTick(function() {
          this.getCharts2()// 通过axios拿到的后台数据填充的echarts图表方法
        })
      } else {
        this.$nextTick(function() {
          this.getCharts3()
        })
      }
    },
    handleClick(data) {
      console.log(data)
      this.treeObj.ppidLabel = data.ppidLabel
      this.treeObj.pidLabel = data.pidLabel
      this.treeObj.label = data.label
    },
    saveDwObj(data) {
      this.treeObj.pppidLabel = data
    },
    fn(data) {

    },
    getCurrentRow(row) {
      console.log('11', row)
      this.cjDate = row.date
    },
    // 每页显示的条数改变
    changeSizeHandle(val) {
      this.size = val
      this.currentPage = 1
      // this.getInfo()
      console.log(`每页 ${val} 条`)
    },

    // current-change用于监听页数改变，而内容也发生改变
    currentChangeHandle(val) {
      this.currentPage = val
      // this.getInfo()
      console.log(`当前页: ${val}`)
    },
    getCharts() {
      this.lineData = [
        {
          name: 'pinpu',
          data: [
            {
              name: '2020-02-23',
              value: '1'
            },
            {
              name: '2020-02-24',
              value: '1'
            },
            {
              name: '2020-02-25',
              value: '1'
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
      ]
      for (let p = 0; p < this.lineData[0].data.length; p++) {
        this.xAxisDataLline.push(this.lineData[0].data[p].name)
      }
    },
    getCharts2() {
      this.lineData2 = [
        {
          name: 'pinpu',
          data: [
            {
              name: '2020-02-23',
              value: '1'
            },
            {
              name: '2020-02-24',
              value: '1'
            },
            {
              name: '2020-02-25',
              value: '1'
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
      ]
      for (let p = 0; p < this.lineData2[0].data.length; p++) {
        this.xAxisDataLline2.push(this.lineData2[0].data[p].name)
      }
    },
    getCharts3() {
      this.lineData3 = [
        {
          name: 'pinpu',
          data: [
            {
              name: '2020-02-23',
              value: '1'
            },
            {
              name: '2020-02-24',
              value: '1'
            },
            {
              name: '2020-02-25',
              value: '1'
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
      ]
      for (let p = 0; p < this.lineData3[0].data.length; p++) {
        this.xAxisDataLline3.push(this.lineData3[0].data[p].name)
      }
    }
  }
}
</script>
<style lang="scss" scoped>
    .first-screen-center{
        box-shadow: none;
    }
    .subText{
        color: #3A71A8;
    }
    .first-screen-center-info{
        /*justify-content: center;*/
        /*align-items: center;*/
        width: 90%;
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
</style>
