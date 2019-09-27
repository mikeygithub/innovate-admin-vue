<template>
  <el-dialog
    :title="'批量添加学院用户'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="6rem">
      <el-alert
        title="文件要求"
        type="info"
        description="以附件形式上传修改好的数据模板，注意检查是否存在重复数据！"
        show-icon
        :closable="false">
      </el-alert>
      <el-form-item label="录入数据">
        <el-upload
          class="upload-demo"
          ref="upload"
          accept=".xls,.xlsx"
          action="https://jsonplaceholder.typicode.com/posts/"
          :on-change="upload"
          :show-file-list="false"
          :auto-upload="false">
          <el-button slot="trigger" size="small" icon="el-icon-upload" type="primary">点击上传</el-button>
        </el-upload>
      </el-form-item>
    </el-form>
    <template v-if="errorDataCode">
      <el-alert
        title="格式错误数据"
        type="error"
        description="以下要录入的用户数据格式有误或数据中存在已录入的用户数据，请认真检查并重新上次他们的数据！未显示在下面用户数据已保存成功，无需从新填写！"
        show-icon
        :closable="false">
      </el-alert>
    </template>
    <table border="1" cellspacing="0" width="100%" class="table" id="out-table">
      <tr align='center'>
        <th>序号</th>
        <th>账号</th>
        <th>姓名</th>
        <th>手机号</th>
        <th>邮箱</th>
        <th>用户类型</th>
      </tr>
      <template>
        <tr align='center' v-if="batchSaveList.length === 0">
          <td>暂无数据</td>
          <td>暂无数据</td>
          <td>暂无数据</td>
          <td>暂无数据</td>
          <td>暂无数据</td>
          <td>暂无数据</td>
        </tr>
      </template>
      <template>
        <tr v-for="(item,index) in batchSaveList" align="center">
          <td v-text="index+1"></td>
          <td><span v-text="item.__EMPTY"></span></td>
          <td><span v-text="item.__EMPTY_1"></span></td>
          <td><span v-text="item.__EMPTY_2"></span></td>
          <td><span v-text="item.__EMPTY_3"></span></td>
          <td>
            <span v-for="role in roleList" v-if="item.__EMPTY_4 === role.roleId" v-text="role.roleName"></span>
          </td>
        </tr>
      </template>
    </table>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false, errorDataCode = false, batchSaveList = []">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()" :loading="registerLoading">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  import XLSX from 'xlsx'
  export default {
    data () {
      return {
        visible: false,
        instituteList: this.$store.state.user.institute,
        roleList: [
          {roleId: 2, roleName: '项目负责人'},
          {roleId: 3, roleName: '指导老师'}
        ],
        batchSaveList: [],
        batchSaveLoading: false,
        errorDataCode: false,
        dataForm: {
          id: 0,
          name: '',
          username: '',
          password: '',
          comfirmPassword: '',
          salt: '',
          email: '',
          mobile: '',
          instituteId: '',
          roleIdList: [],
          status: 1
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.registerLoading = true
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/sys/user/batchsave`),
              method: 'post',
              data: this.$http.adornData({
                'erInstituteId': this.$store.state.user.instituteId,
                'batchSaveList': this.batchSaveList
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.registerLoading = false
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
                // this.$message.error(data.msg)
                this.batchSaveList = data.errorDate
                this.errorDataCode = true
                this.registerLoading = false
              }
            })
          }
        })
      },
      upload (file, fileList) {
        this.errorDataCode = false
        let files = {0: file.raw}
        this.readExcel1(files)
      },
      // 表格导入
      readExcel1 (files) {
        // 如果没有文件名
        if (files.length <= 0) {
          return false
        } else if (!/\.(xls|xlsx)$/.test(files[0].name.toLowerCase())) {
          this.$Message.error('上传格式不正确，请上传xls或者xlsx格式')
          return false
        }

        const fileReader = new FileReader()
        fileReader.onload = (ev) => {
          try {
            const data = ev.target.result
            // 以二进制流方式读取得到整份excel表格对象
            const workbook = XLSX.read(data, {
              type: 'binary'
            })
            // 取第一张表
            const wsname = workbook.SheetNames[0]
            // 生成json表格内容
            const ws = XLSX.utils.sheet_to_json(workbook.Sheets[wsname])
            if (ws.length <= 2) {
              this.$message.error('请填写用户信息,不能上传空文件！')
              return false
            }
            // 重写数据
            try {
              this.batchSaveList = ws.slice(2, ws.length)
            } catch (err) {
              this.$message.error(err)
            }
            this.$refs.upload.value = ''
          } catch (e) {
            return false
          }
        }
        fileReader.readAsBinaryString(files[0])
      }
    }
  }
</script>
