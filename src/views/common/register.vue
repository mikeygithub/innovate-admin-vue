<template>
  <el-dialog
    :title="'注册'"
    @close="closeRegister"
    :close-on-click-modal="false"
    width="50rem"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" status-icon>
      <el-form-item prop="username">
        <el-input v-model="dataForm.username" placeholder="帐号"></el-input>
      </el-form-item>
      <el-form-item prop="name">
        <el-input v-model="dataForm.name" placeholder="姓名"></el-input>
      </el-form-item>
      <el-form-item prop="userPhone">
        <el-input v-model="dataForm.userPhone" placeholder="手机号"></el-input>
      </el-form-item>
      <el-form-item prop="userEmail">
        <el-input v-model="dataForm.userEmail" placeholder="邮箱"></el-input>
      </el-form-item>
      <el-form-item prop="userPassword">
        <el-input v-model="dataForm.userPassword" type="password" placeholder="密码"></el-input>
      </el-form-item>
      <el-form-item prop="surePassword">
        <el-input v-model="dataForm.surePassword" type="password" placeholder="确认密码"></el-input>
      </el-form-item>
      <el-form-item prop="instituteId">
        <el-select v-model="dataForm.instituteId" placeholder="请选择所属部门">
          <el-option v-for="item in instituteList" :key="item.instituteId" :label="item.instituteName"
                     :value="item.instituteId">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="captcha">
        <el-row :gutter="24">
          <el-col :span="14">
            <el-input v-model="dataForm.captcha" placeholder="请输入验证码"></el-input>
          </el-col>
          <el-col :span="8">
            <el-button @click="getCaptchaSubmit()" type="success" :disabled="codeDisable" v-if="codeDisable==false">获取验证码</el-button>
            <el-button @click="getCaptchaSubmit()" type="success" :disabled="codeDisable" v-if="codeDisable==true">{{codeSendMsg}}</el-button>
          </el-col>
        </el-row>
      </el-form-item>
      <el-form-item prop="type" style="text-align: center">
        <template>
          <el-row>
            <el-col :span="11" :offset="1">
              <el-radio v-model="dataForm.type" label="1" border style="width: 100%">学生</el-radio>
            </el-col>
            <el-col :span="11" :offset="1">
              <el-radio v-model="dataForm.type" label="2" border style="width: 100%">教师</el-radio>
            </el-col>
          </el-row>
        </template>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()" :loading="registerLoading">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  import {isEmail, isMobile} from '@/utils/validate'

  export default {
    data () {
      var validateMobile = (rule, value, callback) => {
        if (!isMobile(value)) {
          callback(new Error('手机号格式错误'))
        } else {
          callback()
        }
      }
      var validateEmail = (rule, value, callback) => {
        if (!isEmail(value)) {
          callback(new Error('邮箱格式错误'))
        } else {
          callback()
        }
      }
      var validateSurePassword = (rule, value, callback) => {
        if (this.dataForm.userPassword !== this.dataForm.surePassword) {
          callback(new Error('密码不一致'))
        } else {
          callback()
        }
      }
      return {
        codeSendMsg: '重新发送',
        time: 0,
        visible: false,
        loading: false,
        codeDisable: false,
        registerLoading: false,
        instituteList: this.$store.state.user.institute,
        dataForm: {
          id: 0,
          username: '',
          name: '',
          userPassword: '',
          surePassword: '',
          instituteId: '',
          userPhone: '',
          userEmail: '',
          type: '',
          captcha: ''
        },
        dataRule: {
          username: [
            {required: true, message: '账号不能为空', trigger: 'blur'}
          ],
          name: [
            {required: true, message: '姓名不能为空', trigger: 'blur'}
          ],
          userEmail: [
            {required: true, message: '邮箱不能为空', trigger: 'blur'},
            {validator: validateEmail, trigger: 'blur'}
          ],
          instituteId: [
            {required: true, message: '所属部门不能为空', trigger: 'blur'}
          ],
          userPhone: [
            {required: true, message: '手机号不能为空', trigger: 'blur'},
            {validator: validateMobile, trigger: 'blur'}
          ],
          userPassword: [
            {required: true, message: '密码不能为空', trigger: 'blur'}
          ],
          surePassword: [
            {required: true, message: '确认密码不能为空', trigger: 'blur'},
            {validator: validateSurePassword, trigger: 'blur'}
          ],
          type: [
            {required: true, message: '角色不能为空', trigger: 'blur'}
          ],
          captcha: [
            {required: true, message: '验证码不能为空', trigger: 'blur'}
          ]
        }
      }
    },
    methods: {
      init () {
        this.$http({
          url: this.$http.adornUrl('/innovate/sys/institute/all'),
          method: 'get',
          params: this.$http.adornParams()
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.$store.state.user.institute = data.institute
            this.visible = true
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      getCaptchaSubmit () {
        if (isMobile(this.dataForm.userPhone)) {
          this.codeDisable = true
          this.time = 60
          this.timer()
          this.$http({
            url: this.$http.adornUrl(`/sys/message`),
            method: 'post',
            data: this.$http.adornData({
              'mobile': this.dataForm.userPhone
            })
          }).then(({data}) => {
            this.$message.success('发送短信成功')
            console.info(data)
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
      // 表单提交
      dataFormSubmit () {
        this.registerLoading = true
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/sys/register`),
              method: 'post',
              data: this.$http.adornData({
                'username': this.dataForm.username,
                'name': this.dataForm.name,
                'password': this.dataForm.userPassword,
                'instituteId': this.dataForm.instituteId,
                'mobile': this.dataForm.userPhone,
                'email': this.dataForm.userEmail,
                'type': this.dataForm.type,
                'captcha': this.dataForm.captcha
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
                this.registerLoading = false
              }
            })
          }
        })
      },
      closeRegister () {
        this.visible = false
        this.$emit('refreshDataList')
      }
    }
  }
</script>
