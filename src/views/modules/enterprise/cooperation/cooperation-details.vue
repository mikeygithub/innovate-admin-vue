<template>
  <el-dialog
    :title="'详情'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" label-width="80px">
      <el-form-item label="项目名称" prop="proName">
        <el-input v-model="dataForm.proName" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="合作内容" prop="cooperationContent">
        <el-input v-model="dataForm.cooperationContent" placeholder="合作内容"></el-input>
      </el-form-item>
      <el-form-item label="合作方式" prop="cooperationType">
        <el-input v-model="dataForm.cooperationType" placeholder="合作方式"></el-input>
      </el-form-item>
      <el-form-item label="合作要求" prop="cooperationRequire">
        <el-input v-model="dataForm.cooperationRequire" placeholder="合作要求"></el-input>
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
        proName: '',
        proCooperationInfoId: 0,
        proInfoId: '',
        cooperationContent: '',
        cooperationType: '',
        cooperationRequire: '',
        userPerId: '',
        userTeacherId: '',
        entInfoId: '',
        inApply: ''
      }
    }
  },
  methods: {
    init (id) {
      this.dataForm.proCooperationInfoId = id || 0
      this.visible = true
      this.$nextTick(() => {
        this.$refs['dataForm'].resetFields()
        if (this.dataForm.proCooperationInfoId) {
          this.$http({
            url: this.$http.adornUrl(`/enterprise/entprojectcooperationinfo/info/${this.dataForm.proCooperationInfoId}`),
            method: 'get',
            params: this.$http.adornParams()
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.dataForm.proInfoId = data.entprojectcooperationinfo.proInfoId
              this.dataForm.cooperationContent = data.entprojectcooperationinfo.cooperationContent
              this.dataForm.cooperationType = data.entprojectcooperationinfo.cooperationType
              this.dataForm.cooperationRequire = data.entprojectcooperationinfo.cooperationRequire
              this.dataForm.userPerId = data.entprojectcooperationinfo.userPerId
              this.dataForm.userTeacherId = data.entprojectcooperationinfo.userTeacherId
              this.dataForm.entInfoId = data.entprojectcooperationinfo.entInfoId
              this.dataForm.inApply = data.entprojectcooperationinfo.inApply
            }
          })
        }
      })
    }
  }
}
</script>
