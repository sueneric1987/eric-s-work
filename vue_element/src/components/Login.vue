<template>
  <div>
    <img src="../assets/logo.png" ></img>
    <el-form class="login-box" status-icon :rules="loginRules" ref="loginForm" :model="loginForm" >
      <h3 class="login-title">欢迎登录</h3>
      <el-form-item prop="username">
        <el-input  size="small" @keyup.enter.native="handleLogin" v-model="loginForm.username" auto-complete="off" placeholder="请输入用户名">
          <i slot="prepend" class="el-icon-user"></i>
        </el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input size="small" @keyup.enter.native="handleLogin" :type="passwordType" v-model="loginForm.password" auto-complete="off" placeholder="请输入密码">
          <i class="el-icon-view el-input__icon" :style="fontstyle" slot="suffix" @click="showPassword"></i>
          <i slot="prepend" class="el-icon-lock"></i>
        </el-input>
      </el-form-item>
      <el-form-item  prop="verifycode">
        <!-- 注意：prop与input绑定的值一定要一致，否则验证规则中的value会报undefined，因为value即为绑定的input输入值 -->
        <el-input v-model="loginForm.verifycode" placeholder="请输入验证码" class="identifyinput"></el-input>
      </el-form-item>
      <el-form-item>
        <div class="identifybox">
          <div @click="refreshCode">
            <sidentify :identifyCode="identifyCode"></sidentify>
          </div>
          <el-button @click="refreshCode" type='text' class="textbtn">看不清，换一张</el-button>
        </div>
      </el-form-item>
      <el-checkbox v-model="checked">记住账号</el-checkbox>
      <el-form-item>
        <el-button type="primary" size="small" @click.native.prevent="handleLogin" class="login-submit">登录</el-button>
      </el-form-item>
    </el-form>
    <span>© 2019 All Rights Reserved ⋅ Phoenix IT </span>
  </div>
</template>
<script>
import sidentify from '@/components/Identify'
export default {
  name: 'userlogin',
  data() {
    // 用户名自定义验证规则
    const validateUsername = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入正确的用户名'))
      } else {
        console.log('user', value)
        callback()
      }
    }
    // 验证码自定义验证规则
    const validateVerifycode = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入验证码'))
      } else if (value.toLowerCase() !== this.identifyCode.toLowerCase()) {
        console.log('validateVerifycode:', value)
        callback(new Error('验证码不正确!'))
      } else {
        callback()
      }
    }
    return {
      fontstyle: {
      },
      loginForm: {
        username: 'admin',
        password: '123456',
        verifycode: ''
      },
      checked: false,
      identifyCodes: 'AaBbCcDdEeFfGgHhIiJjKkLlMmNnOoPpQqRrSsTtUuVvWwXxYyZz',
      identifyCode: '',
      loginRules: { // 绑定在form表单中的验证规则
        username: [
          { required: true, trigger: 'blur', validator: validateUsername }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, message: '密码长度最少为6位', trigger: 'blur' }
        ],
        verifycode: [
          { required: true, trigger: 'blur', validator: validateVerifycode }
        ]
      },
      passwordType: 'password'
    }
  },
  components: {
    sidentify
  },
  created() {
  },
  mounted() {
    // 验证码初始化
    this.identifyCode = ''
    this.makeCode(this.identifyCodes, 4)
  },
  computed: {
  },
  props: [],
  methods: {
    // 通过改变input的type使密码可见
    showPassword() {
      this.fontstyle === '' ? (this.fontstyle = 'color: red') : (this.fontstyle = '') // 改变密码可见按钮颜色
      this.passwordType === ''
        ? (this.passwordType = 'password')
        : (this.passwordType = '')
    },
    // 点击登入按钮
    handleLogin() {
      this.$refs.loginForm.validate(valid =>{
        if(valid){
          this.$router.push('/order');
        }else{
          alert('error');
        }
      })
    },
    // 生成随机数
    randomNum(min, max) {
      return Math.floor(Math.random() * (max - min) + min)
    },
    // 切换验证码
    refreshCode() {
      this.identifyCode = ''
      this.makeCode(this.identifyCodes, 4)
    },
    // 生成四位随机验证码
    makeCode(o, l) {
      for (let i = 0; i < l; i++) {
        this.identifyCode += this.identifyCodes[
          this.randomNum(0, this.identifyCodes.length)
        ]
      }
      console.log(this.identifyCode)
    }
  }
}
</script>
<style scoped>
.login-box {
    border: 1px solid #DCDFE6;
    width: 350px;
    margin: 40px auto;
    padding: 35px 35px 15px 35px;
    border-radius: 5px;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    box-shadow: 0 0 25px #909399;
  }
.identifybox{
  display: flex;
  justify-content: space-between;
  margin-top:7px;
}
.iconstyle{
  color:#409EFF;
}
</style>
