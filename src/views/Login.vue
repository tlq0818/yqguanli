<template>
  <div class="wrapper">
    <div style="margin: 200px auto; background-color: #fff; width: 350px; height: 300px; padding: 20px; border-radius: 10px">
      <div style="margin: 20px 0; text-align: center; font-size: 24px"><b>登 录</b></div>
      <el-form :model="user" :rules="rules" ref="userForm">
        <el-form-item prop="xingming">
          <el-input size="medium" style="margin: 10px 0" prefix-icon="el-icon-user" v-model="user.xingming"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input size="medium" style="margin: 10px 0" prefix-icon="el-icon-lock" show-password v-model="user.password"></el-input>
<!--          show-password:点击小眼睛可以看到密码-->
        </el-form-item>
        <el-form-item style="margin: 10px 0; text-align: right">
          <el-button type="primary" size="small"  autocomplete="off" @click="login">登录</el-button>
          <el-button type="warning" size="small"  autocomplete="off" @click="$router.push('/register')">注册</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      user: {},
      rules: {
        xingming: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 1, max: 10, message: '长度在 1到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 1, max: 20, message: '长度在 1 到 20 个字符', trigger: 'blur' }
        ],
      }
    }
  },
  methods: {
    login() {
      this.$refs['userForm'].validate((valid) => {
        if (valid) {  // 表单校验合法
          this.request.post("http://localhost:9090/user/login", this.user).then(res => {
            if(res.code==="200") {
              localStorage.setItem("user",JSON.stringify(res.data)) // 存储用户信息 到浏览器
              this.$router.push("/")
              this.$message.success("登录成功，欢迎来到本系统")
            } else {
              this.$message.error(res.msg)
            }
          })
        }
      });
    }
  }
}
</script>

<style>
.wrapper {
  height: 100vh;
  width: 100%;
  background-image: url("../assets/bg.jpg");
  background-size: cover;
  position: fixed;
}
</style>
