<template>
  <div class="LoginContainer">
    <div class="LoginBox">
      <!-- 头像区域 -->
      <div class="imgContain">
        <img src="../assets/Company.png" alt />
      </div>
      <!-- 文本区域 -->
      <el-form
        ref="loginFormRef"
        label-width="0px"
        class="login_from"
        :model="LoginForm"
        :rules="LoginFormRules"
      >
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input prefix-icon="el-icon-user-solid" v-model="LoginForm.username"></el-input>
        </el-form-item>
        <!-- 密码区域 -->
        <el-form-item prop="password">
          <el-input
            placeholder="请输入密码"
            show-password
            prefix-icon="el-icon-message-solid"
            v-model="LoginForm.password"
          ></el-input>
        </el-form-item>
        <!-- 按钮区域 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info">注册</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      LoginForm: {
        username: 'admin',
        password: '123456'
      },
      LoginFormRules: {
        // 设置输入规则
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 10, message: '用户名长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { min: 3, max: 10, message: '密码长度在 3 到 10 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 点击登录按钮前，再次进行数据格式的验证，再发起请求 validate bool
    login () {
      // 登录前预校验 返回的Promise结构，使用异步获取(async + await)
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return false
        // 解析出返回的data数据
        // message消息提示
        const { data: res } = await this.$http.post('login', this.LoginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败')
        // 把token保存到seesionStorage(默认有效时间)
        window.sessionStorage.setItem('token', res.data.token)
        this.$message({
          message: '登陆成功',
          type: 'success'
        })
        // 通过编程式跳转到后台主页， /home
        this.$router.push('/home')
      })
    }
  }
  // watch: {
  //   'LoginForm.username': function () {
  //     console.log(this.LoginForm.username)
  //   }
  // }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.LoginContainer {
  background-color: #2b4b6b;
  width: 100%;
  height: 100%;
}
.LoginBox {
  background-color: #fff;
  height: 300px;
  width: 450px;
  border: 0 solid 1em;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.imgContain {
  width: 130px;
  height: 130px;
  padding: 10px;
  background-color: #fff;
  border-radius: 50%;
  box-shadow: 0 0 10px #ddd;
  left: 225px;
  position: absolute;
  transform: translate(-50%, -50%);
  img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eee;
  }
}
.login_from {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
.btns {
  display: flex;
  justify-content: center;
}
</style>
