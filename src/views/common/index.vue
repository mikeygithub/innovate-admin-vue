<template>
  <div class="site-wrapper site-page--login">
    <div class="site-content__wrapper">
      <div class="site-content">
        <div class="brand-info">
          <h2 class="brand-info__text">校企合作及创新创业管理平台</h2>
          <p class="brand-info__intro">校企合作及创新创业管理平台基于element-ui构建开发，实现后台管理前端功能，提供一套更优的前端解决方案。</p>
        </div>
        <div>
          <el-form>
            <el-form-item>
              <el-row :gutter="38">
                <el-col :span="6">
                  <el-button class="login-btn-submit" type="primary" @click="loginSubmit()">登录</el-button>
                </el-col>
                <el-col :span="6">
                  <el-button class="login-btn-submit" type="primary" @click="logoutHandle()">退出登录</el-button>
                </el-col>
                <el-col :span="6">
                  <el-button class="login-btn-submit" type="primary" @click="registerSubmit()">注册</el-button>
                </el-col>
                <el-col :span="6">
                  <el-button class="login-btn-submit" type="primary" @click="openMainPage">后台管理系统</el-button>
                </el-col>
              </el-row>
            </el-form-item>
          </el-form>
        </div>
      </div>
    </div>
    <Register v-if="registerVisible" ref="register" @refreshDataList="getCaptcha"></Register>
    <Login v-if="loginVisible" ref="login" @refreshDataList="getCaptcha"></Login>
  </div>

</template>

<script>
  import {getUUID, clearLoginInfo} from '@/utils'
  import Register from './register'
  import Login from './login'

  export default {
    data () {
      return {
        registerVisible: false,
        loginVisible: false,
        dataForm: {
          userName: '',
          password: '',
          uuid: '',
          captcha: ''
        },
        dataRule: {
          userName: [
            {required: true, message: '帐号不能为空', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '密码不能为空', trigger: 'blur'}
          ],
          captcha: [
            {required: true, message: '验证码不能为空', trigger: 'blur'}
          ]
        },
        captchaPath: ''
      }
    },
    created () {
      this.getCaptcha()
    },
    components: {
      Register,
      Login
    },
    methods: {
      // 提交表单
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl('/sys/login'),
              method: 'post',
              data: this.$http.adornData({
                'username': this.dataForm.userName,
                'password': this.dataForm.password,
                'uuid': this.dataForm.uuid,
                'captcha': this.dataForm.captcha
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$cookie.set('token', data.token)
                this.$router.replace({name: 'home'})
              } else {
                this.getCaptcha()
                this.$message.error(data.msg)
              }
            })
          }
        })
      },
      loginSubmit () {
        // 未登入
        if (this.$cookie.get('token') === null) {
          this.loginVisible = true
          this.$nextTick(() => {
            this.$refs.login.init()
          })
        } else {
          this.$message.error('您已登入,无需重复登入！')
        }
      },
      openMainPage () {
        if (this.$cookie.get('token') === null) {
          this.$message.error('请先登入')
        } else {
          this.$router.replace({name: 'home'})
        }
      },
      registerSubmit () {
        this.registerVisible = true
        this.$nextTick(() => {
          this.$refs.register.init()
        })
      },
      // 获取验证码
      getCaptcha () {
        this.dataForm.uuid = getUUID()
        this.captchaPath = this.$http.adornUrl(`/captcha.jpg?uuid=${this.dataForm.uuid}`)
      },
      // 退出
      logoutHandle () {
        if (this.$cookie.get('token') === null) {
          this.$message.info('您还未登入')
          return
        }
        this.$confirm(`确定进行[退出]操作?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl('/sys/logout'),
            method: 'post',
            data: this.$http.adornData()
          }).then(({data}) => {
            if (data && data.code === 0) {
              clearLoginInfo()
              this.$message.success('账号已退出')
              this.$router.push({ name: 'index' })
            }
          })
        }).catch(() => {})
      }
    }
  }
</script>

<style lang="scss">
  .site-wrapper.site-page--login {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: rgba(38, 50, 56, .6);
    overflow: hidden;

    &:before {
      position: fixed;
      top: 0;
      left: 0;
      z-index: -1;
      width: 100%;
      height: 100%;
      content: "";
      background-image: url(~@/assets/img/login_bg.jpg);
      background-size: cover;
    }

    .site-content__wrapper {
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      padding: 0;
      margin: 0;
      overflow-x: hidden;
      overflow-y: auto;
      background-color: transparent;
    }

    .site-content {
      min-height: 100%;
      padding: 30px 500px 30px 30px;
    }

    .brand-info {
      margin: 220px 100px 0 90px;
      color: #fff;
    }

    .brand-info__text {
      margin: 0 0 22px 0;
      font-size: 48px;
      font-weight: 400;
      text-transform: uppercase;
    }

    .brand-info__intro {
      margin: 10px 0;
      font-size: 16px;
      line-height: 1.58;
      opacity: .6;
    }

    .login-main {
      position: absolute;
      top: 0;
      right: 0;
      padding: 150px 60px 180px;
      width: 470px;
      min-height: 100%;
      background-color: #fff;
    }

    .login-title {
      font-size: 16px;
    }

    .login-captcha {
      overflow: hidden;

      > img {
        width: 100%;
        cursor: pointer;
      }
    }

    .login-btn-submit {
      width: 100%;
      margin-top: 38px;
    }
  }
</style>
