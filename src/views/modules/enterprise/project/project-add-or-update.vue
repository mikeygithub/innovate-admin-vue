<template>
  <el-dialog
    width="60%"
    :title="!dataForm.id ? '新增项目' : '修改项目'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-row>
      <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="10rem" style="width: 94%; margin: 0 auto">
        <el-col>
          <el-form-item label="项目名称" prop="proName">
            <el-input v-model="dataForm.proName" placeholder="请输入"></el-input>
          </el-form-item>
        </el-col>
        <el-col>
          <el-form-item label="项目类型" prop="proType">
            <el-select v-model="dataForm.proType"  placeholder="请选择">
              <el-option v-for="item in proTypeList" :key="item.value" :label="item.label" :value="item.value">
              </el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col>
          <el-form-item label="项目介绍" prop="proIntroduce">
          <el-input type="textarea" maxlength="400" :rows="5" v-model="dataForm.proIntroduce" placeholder="请输入"></el-input>
        </el-form-item>
        </el-col>
        <el-col>
          <el-form-item label="项目经费" prop="proOutlay">
            <el-input v-model="dataForm.proOutlay" style="width: 24%" placeholder="请输入"></el-input> 万元
          </el-form-item>
        </el-col>
        <el-col>
          <el-form-item label="项目登记" prop="proRegister">
            <el-input type="textarea" v-model="dataForm.proRegister" placeholder="请输入"></el-input>
          </el-form-item>
        </el-col>
        <el-col>
          <el-form-item label="项目来源" prop="proOrigin">
            <el-input type="textarea" v-model="dataForm.proOrigin" placeholder="请输入"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="24">
          <el-form-item label="项目附件">
            <el-upload
              class="upload-demo"
              :action="url"
              :file-list="dataForm.attachments"
              :on-success="handleChange">
              <el-button size="small" icon="el-icon-upload" type="primary">点击上传</el-button>
            </el-upload>
          </el-form-item>
        </el-col>
      </el-form>
    </el-row>
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
      url: '',
      dataForm: {
        proInfoId: 0,
        proName: '',
        proRegister: '',
        proOrigin: '',
        proOutlay: '',
        proType: '',
        proIntroduce: '',
        inApply: '0',
        attachments: [] // 项目附件
      },
      proTypeList: [
        {value: 1, label: '科研项目'},
        {value: 2, label: '横向项目'},
        {value: 3, label: '企业项目'},
        {value: 1, label: '大创项目'},
        {value: 2, label: '企业招聘'},
        {value: 3, label: '实习项目对接'}
      ],
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
  activated () {
    this.getDataList()
    this.dataForm.ent.newHighZones = '0'
    this.dataForm.type = '1'
    this.dataForm.ent.entStatus = 1
    this.url = this.$http.adornUrl('/common/file/upload')
  },
  mounted () {
    this.dataForm.ent.newHighZones = '0'
    this.dataForm.type = '1'
    this.dataForm.ent.entStatus = 1
    this.url = this.$http.adornUrl('/common/file/upload')
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
            url: this.$http.adornUrl(`/enterprise/project/info/${!this.dataForm.proInfoId ? 'save' : 'update'}`),
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
    },
    handleChange (file, fileList) {
      this.dataForm.attachments = fileList.slice(-3)
          // console.log('结果', this.dataForm.attachments)
          // console.log('结果', this.dataForm.attachments)
    },
    // 上传成功
    successHandle (response, file, fileList) {
      if (response && response.code === 0) {
        // this.dataForm.entTeacherAttachmentEntity = response.entTeacherAttachmentEntity
      } else {
        this.$message.error(response.msg)
      }
    }
  }
}
</script>
