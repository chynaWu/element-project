<template>
  <div class="statistics-main-view" style="height: 100%;">

    <div class="first-screen-center" style="box-shadow: none">
      <div class="first-screen-center-info" style="position: relative">
        <mapEchartsLW id="map-chart" width="100%" height="770px" :map-data="mapData" @clickMap="clickMap" />
        <div class="nationwide-info">
          <div class="nationwide-info-item">
            <div class="nationwide-item-title">
              <div class="nationwide-item-text">皮带机总数</div>
            </div>
            <div class="nationwide-item-box">
              <div class="nationwide-item-box-percentage" style="width: 0%;" />
            </div>
            <div class="nationwide-item-box-val">10</div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import mapEchartsLW from '@/components/Charts/mapEchartsLW'
export default {
  name: 'Dashboard',
  components: { mapEchartsLW },
  data() {
    return {
      mapData: [],
    }
  },
  created() {
  },
  mounted() {
  },

  methods: {
    clickMap(param, jw) {
      console.log('param', param, jw)
      console.log(JSON.parse(jw))
      const zbx = JSON.parse(jw)
      if (parseInt(zbx[0]) == 119 && (parseInt(zbx[1]) == 37 || parseInt(zbx[1]) == 38)) {
        console.log('潍坊港')
        this.$router.push({ path: '/jxInfo', query: { type: '1' }})
      } else if (parseInt(zbx[0]) == 117 && (parseInt(zbx[1]) == 34 || parseInt(zbx[1]) == 35)) {
        console.log('潍焦')
        this.$router.push({ path: '/jxInfo', query: { type: '2' }})
      }
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

  }
}
</script>
<style lang="scss" scoped>
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
</style>
<style>
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
