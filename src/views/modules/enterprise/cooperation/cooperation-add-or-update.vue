<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item  label="项目名称" prop="proName">
        <el-select v-if="!dataForm.id" v-model="value" filterable placeholder="请选择">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        <el-input v-if="dataForm.id" v-model="dataForm.proName" :readonly="true"></el-input>
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
        proCooperationInfoId: 0,
        proInfoId: '',
        cooperationContent: '',
        cooperationType: '',
        cooperationRequire: '',
        userPerId: '',
        userTeacherId: '',
        entInfoId: '',
        inApply: ''
      },
      options: null,
      value: '',
      dataRule: {
        proInfoId: [
                    { required: true, message: '项目信息外键不能为空', trigger: 'blur' }
        ],
        cooperationContent: [
                    { required: true, message: '合作内容不能为空', trigger: 'blur' }
        ],
        cooperationType: [
                    { required: true, message: '合作方式不能为空', trigger: 'blur' }
        ],
        cooperationRequire: [
                    { required: true, message: '合作要求不能为空', trigger: 'blur' }
        ]
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
    },
      // 查询用户对应项目信息
    selectProject () {
      this.$nextTick(() => {
        this.$refs['dataForm'].resetFields()
        if (!this.dataForm.proCooperationInfoId) {
          this.$http({
            url: this.$http.adornUrl(`/enterprise/project/info/queryPeojects`),
            method: 'get',
            params: this.$http.adornParams()
          }).then(({data}) => {
            console.log(data)
            const arr = []
            if (data && data.code === 0) {
              data.data && data.data.forEach(function (item, index, a) {
                arr.push({value: item.proInfoId, label: item.proName})
              })
              this.options = arr
              console.log(this.options)
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
            url: this.$http.adornUrl(`/enterprise/project/cooperation/${!this.dataForm.proCooperationInfoId ? 'save' : 'update'}`),
            method: 'post',
            data: this.$http.adornData({
              'proCooperationInfoId': this.dataForm.proCooperationInfoId || undefined,
              'proInfoId': this.dataForm.proInfoId,
              'cooperationContent': this.dataForm.cooperationContent,
              'cooperationType': this.dataForm.cooperationType,
              'cooperationRequire': this.dataForm.cooperationRequire,
              'userPerId': this.dataForm.userPerId,
              'userTeacherId': this.dataForm.userTeacherId,
              'entInfoId': this.dataForm.entInfoId,
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
