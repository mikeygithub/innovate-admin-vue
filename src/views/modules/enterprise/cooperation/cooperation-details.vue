<template>
  <el-dialog
    :title="'详情'"
    :visible.sync="visible">
    <el-form :model="dataForm" ref="dataForm" label-width="80px">
      <el-form-item label="项目名称" prop="proName">
        <el-input v-model="dataForm.projectInfo.proName" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="合作内容" prop="cooperationContent">
        <el-input v-model="dataForm.cooperationContent" placeholder="合作内容" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="合作方式" prop="cooperationType">
        <el-input v-model="dataForm.cooperationType" placeholder="合作方式" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="合作要求" prop="cooperationRequire">
        <el-input v-model="dataForm.cooperationRequire" placeholder="合作要求" :readonly="true"></el-input>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button type="primary" @click="visible = false">返回</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data () {
    return {
      visible: false,
      proCooperationInfoId: 0,
      dataForm: {
      }
    }
  },
  methods: {
    init (id, hasType) {
      this.dataForm.proCooperationInfoId = id || 0
      this.visible = true
      this.hasType = hasType
      this.$nextTick(() => {
        if (this.dataForm.proCooperationInfoId) {
          this.$http({
            url: this.$http.adornUrl(`/enterprise/project/cooperation/info/${this.dataForm.proCooperationInfoId}/${this.hasType}`),
            params: this.$http.adornParams()
          }).then(({data}) => {
            console.log(data)
            if (data && data.code === 0) {
              this.dataForm = data.data
            }
          })
        }
      })
    }
  }
}
</script>
