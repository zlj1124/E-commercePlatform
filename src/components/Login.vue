<template>
  <div class="login_container">
    <div class='title'>电商后台管理平台</div>
    <div class="login_box">
      <!-- logo区域 -->
      <div class="avatar_box">
        <img src="../assets/tao.png" />
      </div>
      <!-- 登录表单区域 -->
      <el-form
        ref="loginFormRef"
        :model="loginForm"
        :rules="loginFormRules"
        label-width="0px"
        class="login_form"
      >
        <!-- ref="loginFormRef" 这个属性值loginFormRef 即为 表单的引用对象 -->
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input
            v-model="loginForm.password"
            prefix-icon="iconfont icon-3702mima"
            type="password"
          ></el-input>
        </el-form-item>
        <!-- 按钮区域 -->
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
  data() {
    return {
      // 这是登录表单的数据绑定对象
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      // 这是表单的验证规则
      loginFormRules: {
        // 验证用户名是否合法
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        // 验证密码是否合法
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 点击重置按钮，重置登录表单
    resetLoginForm() {
      // console.log(this)
      this.$refs.loginFormRef.resetFields()
    },
    login() {
      this.$refs.loginFormRef.validate(async valid => {
        // console.log(valid)
        // 如果valid为false，则不发起请求
        if (!valid) return
        // await 只能用在被async修饰的方法中
        // 从这个对象中把data属性解构赋值出来，重命名为res
        const { data: res } = await this.$http.post('login', this.loginForm)

        // if (res.meta.status !== 200) {
        //     return this.$message.error('登录失败')
        // } else {
        //     return this.$message.success('登录成功')
        // }
        if (res.meta.status !== 200) return this.$message.error('登录失败')
        this.$message.success('登录成功')

        // console.log(res)

        // 1. 将登录成功之后的token，保存到客户端的sessionStorage中
        //  1.1 项目中除了登录之外的其他API接口，必须在登录之后才能访问
        //  1.2 token只应在当前网站打开期间生肖，所有将token保存在sessionStorage中
        window.sessionStorage.setItem('token', res.data.token)
        // 2. 通过编程式导航跳转到后台主页，路由地址是 /home
        this.$router.push('/users')
      })
    }
  }
}
</script>

<style lang="less" scoped>

.login_container {
  background-color: #2b4b6b;
  height: 100%;
}
.title{
  color:floralwhite;
  font-size:35px;
  letter-spacing:8px;
  text-shadow: 0 0 15px #ddd;
  width:400px;
  margin: 0 auto;

}
.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  /* 位置居于正中间 */
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
   box-shadow: 0 0 15px #ddd;

  /* 顶部logo样式 */
  .avatar_box {
    height: 130px;
    width: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px; /*内边距*/
    box-shadow: 0 0 10px #ddd;

    /* logo位置处理 */
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);

    background-color: #fff;

    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
}

.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}

.btns {
  display: flex;
  // justify-content: flex-end;
  justify-content: center;
}
</style>
