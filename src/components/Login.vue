<template>
  <div class="login_box">
    <div class="login_form">
      <div class="logo">
        <img src="../assets/logo.png" alt="">
      </div>
      <!--登录表单-->
      <el-form ref="loginFormRef" label-width="0px" :model="loginForm" :rules="loginRules">
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="el-icon-user-solid"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="el-icon-lock"></el-input>
        </el-form-item>
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      // 登录表单的数据绑定对象
      loginForm: {
        username: 'lisi',
        password: '123456'
      },
      // 表单验证规则对象
      loginRules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 15, message: '长度在 3 到 15 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 12, message: '长度在 6 到 12 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 重置表单功能
    resetLoginForm () {
      this.$refs.loginFormRef.resetFields()
    },
    // 登录处理
    login () {
      this.$refs.loginFormRef.validate(async (valid) => {
        if (!valid) return
        // 发请求
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) {
          return this.$message({
            message: '登录失败',
            type: 'error'
          })
        }
        this.$message.success('登录成功')
        // 1 把token保存起来
        window.sessionStorage.setItem('token', res.data.token)
        // 2 跳转主页
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
  .login_box {
    height: 100%;
    background-color: #2b4b6b;
  }

  .login_form{
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 4px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
  }

  .login_form .logo{
    position: absolute;
    left: 50%;
    transform: translate(-50%,-50%);
    width: 140px;
    height: 140px;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 0 0 10px #ddd;
    background-color: #fff;
    img{
      width: 100%;
      height: 100%;
      border-radius: 50%;
    }
  }

  .login_form .el-form{
    position: absolute;
    bottom: 0;
    padding: 0 20px ;
    width: 100%;
    box-sizing: border-box;
  }

  .btns{
    display: flex;
    justify-content: flex-end;
  }
</style>
