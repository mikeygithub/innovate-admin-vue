<template>
  <el-dialog
    :title="'账号/密码找回'"
    :close-on-click-modal="false"
    :visible.sync="visible">
      <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px" status-icon>
        <el-form-item label="手机号码" prop="userName">
          <el-row :gutter="20">
            <el-col :span="10">
              <el-input v-model="dataForm.mobile" placeholder="请输入手机号"></el-input>
            </el-col>
            <el-col :span="10">
              <el-button @click="getCaptchaSubmit()" type="success" :disabled="codeDisable" v-if="codeDisable==false">获取验证码</el-button>
              <el-button @click="getCaptchaSubmit()" type="success" :disabled="codeDisable" v-if="codeDisable==true">{{codeSendMsg}}</el-button>
            </el-col>
          </el-row>
        </el-form-item >
        <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px" v-if="codeDiv==true">
          <el-form-item label="账号">
            <span>{{ username }}</span>
          </el-form-item>
          <el-form-item label="新密码" prop="newPassword">
            <el-input type="password" v-model="dataForm.newPassword"></el-input>
          </el-form-item>
          <el-form-item label="确认密码" prop="confirmPassword">
            <el-input type="password" v-model="dataForm.confirmPassword"></el-input>
          </el-form-item>
          <el-form-item label="验证码" prop="confirmPassword">
            <el-row gutter="10">
              <el-col span="10">
                <el-input type="text" v-model="dataForm.captcha"></el-input>
              </el-col>
            </el-row>
          </el-form-item>
        </el-form>
      </el-form>
      <span slot="footer" class="dialog-footer" v-if="codeDiv==true">
        <el-button @click="visible = false">取消</el-button>
        <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
      </span>
  </el-dialog>
</template>

<script>
    import {isMobile} from '@/utils/validate'

    export default {
      data () {
        return {
          visible: false,
          registerVisible: false,
          loginLoading: false,
          codeDisable: false,
          codeDiv: false,
          codeSendMsg: '重新发送',
          username: '',
          time: 0,
          user: {},
          dataForm: {
            mobile: '',
            password: '',
            uuid: '',
            captcha: '',
            newPassword: '',
            confirmPassword: ''
          },
          instituteList: this.$store.state.user.institute,
          dataRule: {
            mobile: [
              { required: true, message: '帐号/手机号不能为空', trigger: 'blur' }
            ],
            password: [
              { required: true, message: '密码不能为空', trigger: 'blur' }
            ],
            captcha: [
              { required: true, message: '验证码不能为空', trigger: 'blur' }
            ]
          },
          captchaPath: ''
        }
      },
      components: {
      },
      created () {
      },
      mounted () {
      },
      methods: {
        init () {
          this.visible = true
        },
          // 获取验证码
        getCaptchaSubmit () {
          if (isMobile(this.dataForm.mobile)) {
            this.codeDisable = true
            this.time = 60
            this.timer()
            this.$http({
              url: this.$http.adornUrl(`/webpage/mobile`),
              method: 'post',
              data: this.$http.adornData({
                'mobile': this.dataForm.mobile
              })
            }).then(({data}) => {
              console.info(data)
              this.user = data.user
              console.log(this.user)
              if (this.user === '') {
                this.$message.error('手机号未注册！')
              } else {
                this.$http({
                  url: this.$http.adornUrl(`/sys/message`),
                  method: 'post',
                  data: this.$http.adornData({
                    'mobile': this.dataForm.mobile
                  })
                }).then(({data}) => {
                  this.$message.success('发送短信成功')
                  console.info(data)
                  this.codeDiv = true
                })
              }
            })
          } else {
            this.$message.error('请输入正确的手机号！')
          }
        },
          // 60S倒计时
        timer () {
          if (this.time > 0) {
            this.time--
            this.codeSendMsg = this.time + 's后重新获取'
            setTimeout(this.timer, 1000)
          } else {
            this.time = 0
            this.codeSendMsg = '获取验证码'
            this.codeDisable = false
          }
        },
        registerSubmit () {
          this.registerVisible = true
          this.$nextTick(() => {
            this.$refs.register.init()
          })
        }
      }
    }
</script>

