<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="类型：0 大创,1 中期检查,2 赛事,3 结题" prop="fileAskType">
      <el-input v-model="dataForm.fileAskType" placeholder="类型：0 大创,1 中期检查,2 赛事,3 结题"></el-input>
    </el-form-item>
    <el-form-item label="要求" prop="fileAskContent">
      <el-input v-model="dataForm.fileAskContent" placeholder="要求"></el-input>
    </el-form-item>
    <el-form-item label="年度" prop="fileAskTime">
      <el-input v-model="dataForm.fileAskTime" placeholder="年度"></el-input>
    </el-form-item>
    <el-form-item label="删除标识" prop="isDel">
      <el-input v-model="dataForm.isDel" placeholder="删除标识"></el-input>
    </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          fileAskId: 0,
          fileAskType: '',
          fileAskContent: '',
          fileAskTime: '',
          isDel: ''
        },
        dataRule: {
          fileAskType: [
            { required: true, message: '类型：0 大创,1 中期检查,2 赛事,3 结题不能为空', trigger: 'blur' }
          ],
          fileAskContent: [
            { required: true, message: '要求不能为空', trigger: 'blur' }
          ],
          fileAskTime: [
            { required: true, message: '年度不能为空', trigger: 'blur' }
          ],
          isDel: [
            { required: true, message: '删除标识不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.fileAskId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.fileAskId) {
            this.$http({
              url: this.$http.adornUrl(`/check/innovatefileask/info/${this.dataForm.fileAskId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.fileAskType = data.innovatefileask.fileAskType
                this.dataForm.fileAskContent = data.innovatefileask.fileAskContent
                this.dataForm.fileAskTime = data.innovatefileask.fileAskTime
                this.dataForm.isDel = data.innovatefileask.isDel
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/check/innovatefileask/${!this.dataForm.fileAskId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'fileAskId': this.dataForm.fileAskId || undefined,
                'fileAskType': this.dataForm.fileAskType,
                'fileAskContent': this.dataForm.fileAskContent,
                'fileAskTime': this.dataForm.fileAskTime,
                'isDel': this.dataForm.isDel
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
              }
            })
          }
        })
      }
    }
  }
</script>
