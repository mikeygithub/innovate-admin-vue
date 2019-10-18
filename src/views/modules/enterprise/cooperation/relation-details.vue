<template>
  <el-dialog
    :title="'项目建立关系详情'"
    :close-on-click-modal="false"
    width="70%"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" label-width="80px">


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
      hasType: '',
      dataForm: {
        proName: '',
        proInfoId: '',
        inApply: ''
      }
    }
  },
  methods: {
    init (id, hasType) {
      this.dataForm.proInfoId = id || 0
      this.visible = true
      this.hasType = hasType
      this.$nextTick(() => {
        if (this.dataForm.proInfoId) {
          this.$http({
            url: this.$http.adornUrl(`/enterprise/person/cooperation/info/${this.dataForm.proInfoId}/${this.hasType}`),
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
