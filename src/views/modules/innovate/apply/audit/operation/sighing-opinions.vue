<!--指导老师签署意见-->
<template>
  <el-dialog
    append-to-body
    :title="!id ? '新增' : '审批'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="6rem">
      <el-form-item label="签署意见" prop="sighingOpinion">
        <el-input
          type="textarea"
          :rows="5"
          placeholder="请输入"
          v-model="dataForm.sighingOpinion">
        </el-input>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消审批</el-button>
      <el-button type="primary" @click="dataFormSubmit()" :loading="submitLoading">确定并提交到二级学院</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    name: 'sighing-opinions',
    data () {
      return {
        visible: false,
        loading: false,
        submitLoading: false,
        userMap: [],
        id: 0,
        // apply: '',
        dataForm: {
          sighingOpinion: ''
        },
        // pageIndex: 1,
        // pageSize: 10,
        // totalPage: 0,
        dataRule: {
          sighingOpinion: [
            { required: true, message: '签署意见不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (index, apply) {
        // this.apply = apply
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          this.id = index || 0
          if (this.id) {
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.submitLoading = true
            this.$confirm('此操作将使该项目进入下一个审批流程，是否继续?', '提示', {
              confirmButtonText: '确定',
              cancelButtonText: '取消',
              type: 'warning'
            }).then(() => {
              this.$http({
                url: this.$http.adornUrl(`/innovate/declare/signingopinion/save`),
                method: 'post',
                params: this.$http.adornParams({
                  'declareId': this.id,
                  'userId': this.$store.state.user.id,
                  // 'signingOpinionTime': new Date(),
                  'sighingOpinion': this.dataForm.sighingOpinion,
                  'apply': 'project_audit_apply_status',
                  'roleId': 3
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
                this.submitLoading = false
              })
            }).catch(() => {
              this.$message({
                type: 'info',
                message: '已取消申请'
              })
              this.submitLoading = false
            })
          }
        })
      }
    }
  }
</script>

<style scoped>

</style>
