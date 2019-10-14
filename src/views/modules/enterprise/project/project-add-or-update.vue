<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item label="项目名称" prop="proName">
        <el-input v-model="dataForm.proName" placeholder="项目名称"></el-input>
      </el-form-item>
      <el-form-item label="项目登记" prop="proRegister">
        <el-input type="textarea" v-model="dataForm.proRegister" placeholder="项目登记"></el-input>
      </el-form-item>
      <el-form-item label="项目来源" prop="proOrigin">
        <el-input type="textarea" v-model="dataForm.proOrigin" placeholder="项目来源"></el-input>
      </el-form-item>
      <el-form-item label="项目经费" prop="proOutlay">
        <el-input v-model="dataForm.proOutlay" placeholder="项目经费"></el-input>
      </el-form-item>
      <el-form-item label="项目类型" prop="proType">
        <el-input type="textarea" v-model="dataForm.proType" placeholder="项目类型"></el-input>
      </el-form-item>
      <el-form-item label="项目介绍" prop="proIntroduce">
        <el-input type="textarea" v-model="dataForm.proIntroduce" placeholder="项目介绍"></el-input>
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
        proInfoId: 0,
        proName: '',
        proRegister: '',
        proOrigin: '',
        proOutlay: '',
        proType: '',
        proIntroduce: '',
        inApply: '0'
      },
      dataRule: {
        proName: [
                    { required: true, message: '项目名称不能为空', trigger: 'blur' }
        ],
        proRegister: [
                    { required: true, message: '项目登记不能为空', trigger: 'blur' }
        ],
        proOrigin: [
                    { required: true, message: '项目来源不能为空', trigger: 'blur' }
        ],
        proOutlay: [
                    { required: true, message: '项目经费不能为空', trigger: 'blur' }
        ],
        proType: [
                    { required: true, message: '项目类型不能为空', trigger: 'blur' }
        ],
        proIntroduce: [
                    { required: true, message: '项目介绍不能为空', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    init (id) {
      this.dataForm.proInfoId = id || 0
      this.visible = true
      this.$nextTick(() => {
        this.$refs['dataForm'].resetFields()
        if (this.dataForm.proInfoId) {
          this.$http({
            url: this.$http.adornUrl(`/enterprise/entprojectinfo/info/${this.dataForm.proInfoId}`),
            method: 'get',
            params: this.$http.adornParams()
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.dataForm.proName = data.entprojectinfo.proName
              this.dataForm.proRegister = data.entprojectinfo.proRegister
              this.dataForm.proOrigin = data.entprojectinfo.proOrigin
              this.dataForm.proOutlay = data.entprojectinfo.proOutlay
              this.dataForm.proType = data.entprojectinfo.proType
              this.dataForm.proIntroduce = data.entprojectinfo.proIntroduce
              this.dataForm.entInfoId = data.entprojectinfo.entInfoId
              this.dataForm.userPerId = data.entprojectinfo.userPerId
              this.dataForm.userTeacherId = data.entprojectinfo.userTeacherId
              this.dataForm.inApply = data.entprojectinfo.inApply
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
            url: this.$http.adornUrl(`/enterprise/entprojectinfo/${!this.dataForm.proInfoId ? 'save' : 'update'}`),
            method: 'post',
            data: this.$http.adornData({
              'proInfoId': this.dataForm.proInfoId || undefined,
              'proName': this.dataForm.proName,
              'proRegister': this.dataForm.proRegister,
              'proOrigin': this.dataForm.proOrigin,
              'proOutlay': this.dataForm.proOutlay,
              'proType': this.dataForm.proType,
              'proIntroduce': this.dataForm.proIntroduce,
              'inApply': this.dataForm.inApply
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
