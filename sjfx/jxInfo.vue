<template>
  <div class="statistics-main-view" style="height: 100%;background: none">

    <div class="first-screen-center" style="height: 100%">
      <div class="first-screen-center-info" style="height: 100%">
        <div class="leftBox">
          <!--    :default-expand-all="expandAll"      -->
          <el-tree :data="treeData" :props="defaultProps" node-key="ID"   :default-expanded-keys="defaultExpandedids"  @node-click="handleNodeClick" />
        </div>
        <div class="rightBox">
          <el-tabs type="border-card">
            <el-tab-pane :label="typeItemObj.labelName">
              <div class="infoImg">
                <img v-if="typeItemObj.type=='E'" src="@/assets/dj.jpg" alt="">
                <img v-if="typeItemObj.type=='T'" src="@/assets/qdgt.jpg" alt="">
                <img v-if="typeItemObj.type=='U'" src="@/assets/pdj.png" alt="">
                <img v-if="typeItemObj.type=='J'" src="@/assets/jsj.jpg" alt="">
              </div>
              <table class="show-table">
                <tbody>
                  <tr>
                    <td v-if="typeItemObj.type=='E'" class="title">电机所属集团</td>
                    <td v-if="typeItemObj.type=='T'" class="title">驱动滚筒所属集团</td>
                    <td v-if="typeItemObj.type=='U'" class="title"> 皮带机所属集团</td>
                    <td v-if="typeItemObj.type=='J'" class="title">减速机所属集团</td>
                    <td class="text">顺丰号</td>
                    <td class="title">名称</td>
                    <td class="text">100000</td>
                  </tr>
                  <tr>
                    <td class="title">编号</td>
                    <td class="text">3555</td>
                    <td class="title">类型</td>
                    <td class="text">普通</td>
                  </tr>
                  <tr>
                    <td class="title">出厂日期</td>
                    <td class="text">2020-02-02</td>
                    <td class="title">生产厂家</td>
                    <td class="text">打广告</td>
                  </tr>
                  <tr>
                    <td class="title">其他参数</td>
                    <td class="text">2020-02-02</td>
                    <td class="title" />
                    <td class="text" />
                  </tr>
                </tbody></table>
            </el-tab-pane>
          </el-tabs>
        </div>
      </div>
    </div>
  </div>

</template>

<script>

export default {
  name: 'Dashboard',

  data() {
    return {
      expandAll: true,
      treeData: [],
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
      }
    }
  },
  created() {
    this.type = this.$route.query.type
    this.treeData = [{
      ID: 34,
      Name: '潍焦皮带机',
      Type: 'L',
      children: [{
        ID: 66,
        Name: '1#皮带机',
        Type: 'U',
        children: [{
          // EquipmentNum: 2,
          ID: 7,
          Name: '电机',
          Type: 'E',
          children: [{
            ID: 600003,
            Name: '电机2',
            Number: '600003',
            Type: 'E',
          }, {
            ID: 600002,
            Name: '电机1',
            Number: '600002',
            Type: 'E',
          }]
        }, {
          ID: 8,
          Name: '驱动滚筒',
          Type: 'T',
          children: [{
            ID: 600036,
            Name: '驱动滚筒',
            Number: '600036',
            Type: 'T',
          }, {
            ID: 600004,
            Name: '驱动滚筒',
            Number: '600004',
            Type: 'T',
          }]
        }, {
          ID: 9,
          Name: '皮带机',
          Type: 'U',
          children: [{
            ID: 600066,
            Name: '皮带机',
            Number: '600066',
            Type: 'U',
          }]
        },
        {
          ID: 10,
          Name: '减速机',
          Type: 'J',
          children: [{
            ID: 600086,
            Name: '减速机',
            Number: '600086',
            Type: 'J',
          }]
        }]
      }]
    }]
    this.defaultExpandedids.push(this.treeData[0].ID)
  },
  mounted() {

  },

  methods: {
    handleNodeClick(data) {
      console.log(data)
      this.typeItemObj.labelName = data.Name
      this.typeItemObj.type = data.Type
    }
  }
}
</script>
<style lang="scss" scoped>
 .first-screen-center{
     box-shadow: none;
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
    .el-input--medium .el-input__inner{
        /*height: 20px;*/
        /*line-height: 20px;*/
        /*background-color: #0F355A;*/
        /*margin-left: 70px;*/
        /*width: 240px;*/
    }
    .el-select .el-input .el-select__caret{
        /*display: none;*/
    }
</style>
