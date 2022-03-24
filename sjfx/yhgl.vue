<template>
  <div class="statistics-main-view" style="height: 100%;background: none">

    <div class="first-screen-center" style="box-shadow: none">
      <div
        class="first-screen-center-info"
        style="position: relative;"
      >
        <el-form ref="form" class="user-account-key2" :model="form" :rules="rules" label-width="100px">
          <el-form-item label="用户名" prop="username">
            <el-input v-model="form.username" type="username" placeholder="请输入用户名" maxlength="4" />
          </el-form-item>
          <el-form-item label="密码" prop="password">
            <el-input v-model="form.password" type="password" placeholder="请设置新密码" maxlength="4" />
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="onSubmit('form')">确定</el-button>
            <el-button @click="$refs['form'].resetFields()">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: 'Dashboard',
  data() {
    return {
      form: {
        username: '',
        password: '',
      },
      rules: {
        // 验证规则
        username: [{
          required: true,
          message: '请输入用户名',
          trigger: 'blur'
        }],
        password: [{
          required: true,
          message: '请设置密码',
          trigger: 'blur'
        }, ]
      }
    }
  },
  created() {
    // this.getZhang()
  },
  mounted() {

  },

  methods: {
    getZhang() {
      this.user = JSON.parse(localStorage.getItem('user'))
      this.zhanghao = this.user.username
    },
    onSubmit(formName) {
      const _this = this
      this.$refs[formName].validate(valid => {
        if (valid) {
          const params = {
            username: _this.form.username,
            password: _this.form.password
          }
          this.$api.addUser(params).then(response => {
            if (response.code === '0') {
              _this.$message('新增成功')
              this.$router.push({ path: 'index' })
            } else {
              _this.$message.error(response.message)
            }
          })
        }
      })
    }
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
    .user-account-key2{
        width: 60%;
        margin:0 auto;
        margin-top: 60px;
    }
</style>
<style>
   .user-account-key2 .el-input--medium .el-input__inner{
        background-color: #fff;
        line-height: 40px;
    }
</style>
