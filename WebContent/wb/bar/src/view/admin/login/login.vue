<template>
  <div class="login">
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-position="left" label-width="0px"
             class="demo-ruleForm login-container">
      <h3 class="title">系统登录</h3>
      <el-form-item prop="login">
        <el-input type="text" v-model="ruleForm.login" auto-complete="off" placeholder="账号"></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input
          type="password"
          v-model="ruleForm.password"
          auto-complete="off"
          placeholder="密码"
        ></el-input>
      </el-form-item>
      <el-form-item label="">
        <el-checkbox
          label="记住账号" name="type" v-model="ruleForm.remember"
          style="text-align: left"></el-checkbox>
      </el-form-item>
      <el-form-item>
        <el-button style="width: 100%;" type="primary" @click.native.prevent="submitForm('ruleForm')">登陆
        </el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
  import {LoginApi} from './api'
  export default {
    name: 'Login',
    components: {
      Headers
    },
    /** state 默认信息 */
    data () {
      return {
        ruleForm: {
          login: '',
          password: '',
          remember: false
        },
        rules: {
          login: [
            {required: true, message: '请输入登陆账户', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '请输入登陆密码', trigger: 'change'}
          ]
        }
      }
    },
    /** 计算属性 */
    computed: {},
    mounted () {
      this.getUser()
    },
    /** 方法事件 */
    methods: {
      getUser: function () {
      },
      submitForm: function (formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            let array = {}
            array.method = 'adminLogin'
            array.username = this.ruleForm.login
            array.password = this.ruleForm.password
            LoginApi.Login(array).then(res => {
              if (res.data.status) {
                this.$message({
                  message: res.data.message,
                  type: 'success'
                })
                this.$router.push({path: '/admin'})
              } else {
                this.$message.error(res.data.message)
                return false
              }
            })
          }
        })
      }
    }
  }
</script>

<style lang="scss" scoped>
  .login {
    width: 100%;
    height: 100%;
  }

  .login-container {
    -webkit-border-radius: 5px;
    border-radius: 5px;
    -moz-border-radius: 5px;
    background-clip: padding-box;
    width: 350px;
    padding: 35px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -60%);
    overflow: hidden;
    min-width: 270px;
    .title {
      text-align: center;
      color: #505458;
    }
  }
</style>
