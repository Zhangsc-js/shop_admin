<template>
  <div class="login">
    <el-form ref="form" :model="form" :rules="rules" label-width="80px" status-icon>
      <img src="../assets/avatar.jpg" alt="" >
      <el-form-item label="用户名" prop="username" >
        <el-input v-model="form.username" @keyup.enter.native='login'></el-input>
      </el-form-item>
       <el-form-item label="密码" prop='password'>
        <el-input v-model="form.password" @keyup.enter.native='login'></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" class="loginBtn" @click='login'>登录</el-button>
        <el-button @click='reset'>重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      form: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: ['blur', 'change'] },
          { min: 3, max: 12, message: '用户名长度必须是3-12位', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '密码不能为空', trigger: 'blur' },
          { min: 3, max: 12, message: '密码长度必须在3-12位', trigger: ['change', 'blur'] }
        ]
      }
    }
  },
  methods: {
    reset () {
      this.$refs.form.resetFields()
    },
    async login () {
      try {
        await this.$refs.form.validate()
        const { meta, data } = await this.$axios.post('login', this.form)
        if (meta.status === 200) {
          localStorage.setItem('token', data.token)
          this.$message({
            message: '登录成功',
            type: 'success'
          })
          this.$router.push('/index')
        } else {
          console.log(meta.msg)
          this.$message({
            message: meta.msg,
            type: 'error'
          })
        }
      } catch (e) {
        console.log(e)
      }
    }
  }
}
</script>

<style lang='scss'>
  .login{
    width: 100%;
    height: 100%;
    background-color: #2d434c;
    overflow: hidden;
    .el-form{
      width: 400px;
      background-color: #fff;
      padding: 20px;
      padding-top: 70px;
      border-radius: 20px;
      margin: 0 auto;
      margin-top: 200px;
      position: relative;
      img{
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        top: -75px;
        border: 5px solid #fff;
        border-radius: 50%
      }
      .loginBtn{
        margin-right: 70px;
      }
    }
  }
</style>
