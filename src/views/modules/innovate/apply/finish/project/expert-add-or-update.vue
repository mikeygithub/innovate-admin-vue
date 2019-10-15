<!--添加专家或者更新-->
<template>
  <el-dialog
    append-to-body
    :title="!id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="150px">
      <el-form-item label="专家姓名" prop="perName">
        <el-input v-model="dataForm.perName" placeholder="请输入"></el-input>
      </el-form-item>
      <el-form-item label="职称" prop="perPost">
        <el-input v-model="dataForm.perPost" placeholder="请输入"></el-input>
      </el-form-item>
      <el-form-item label="单位" prop="perPoliticsType">
        <el-input v-model="dataForm.perPoliticsType" placeholder="请输入"></el-input>
      </el-form-item>
      <el-form-item label="联系电话" prop="perTel">
        <el-input v-model="dataForm.perTel" placeholder="请输入"></el-input>
      </el-form-item>
      <el-form-item label="身份证号码" prop="perQq">
        <el-input v-model="dataForm.perQq" placeholder="请输入"></el-input>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  import { isMobile, isEmail } from '@/utils/validate'
  export default {
    name: 'person-add-or-update',
    data () {
      var validateMobile = (rule, value, callback) => {
        if (!isMobile(value)) {
          callback(new Error('手机号格式错误'))
        } else {
          callback()
        }
      }
      return {
        visible: false,
        instituteList: this.$store.state.user.institute,
        gradeList: this.$store.state.user.grade,
        typeList: [{
          value: 1,
          label: '投资'
        }, {
          value: 2,
          label: '管理服务'
        }, {
          value: 3,
          label: '房租和宽带'
        }, {
          value: 4,
          label: '水电费'
        }, {
          value: 5,
          label: '一次性创业'
        }, {
          value: 6,
          label: '吸纳困难群体'
        }, {
          value: 7,
          label: '社会保险'
        }, {
          value: 8,
          label: '创业担保贷款'
        }],
        sexList: [{
          value: 1,
          label: '男'
        }, {
          value: 2,
          label: '女'
        }],
        id: 0,
        dataForm: {
          projectId: '',
          perSex: 0,
          instituteId: '',
          gradeId: '',
          perName: '',
          perPost: '',
          perPoliticsType: '',
          perStuNo: '',
          perClassNo: '',
          perCormNo: '',
          perNative: '',
          perTel: '',
          perQq: '',
          perEmail: '',
          perSchoolPost: '',
          perSchoolHonor: '',
          perSocialPractice: '',
          isDel: 0
        },
        dataRule: {
          perName: [
            { required: true, message: '姓名不能为空', trigger: 'blur' }
          ],
          perEmail: [
            { required: true, message: '邮箱不能为空', trigger: 'blur' },
            { validator: isEmail, trigger: 'blur' }
          ],
          perTel: [
            { required: true, message: '原密码不能为空', trigger: 'blur' },
            { validator: validateMobile, trigger: 'blur' }
          ],
          perSex: [
            { required: true, message: '性别不能为空', trigger: 'blur' }
          ],
          perStuNo: [
            { required: true, message: '学号不能为空', trigger: 'blur' },
            {type: 'number', message: '请输入正确的学号'}
          ],
          instituteId: [
            { required: true, message: '学院不能为空', trigger: 'blur' }
          ],
          gradeId: [
            { required: true, message: '年级不能为空', trigger: 'blur' }
          ],
          perClassNo: [
            { required: true, message: '班级不能为空', trigger: 'blur' }
          ],
          perCormNo: [
            { required: true, message: '宿舍不能为空', trigger: 'blur' }
          ],
          perPost: [
            { required: true, message: '企业职务不能为空', trigger: 'blur' }
          ],
          perNative: [
            { required: true, message: '籍贯不能为空', trigger: 'blur' }
          ],
          perPoliticsType: [
            { required: true, message: '政治面貌不能为空', trigger: 'blur' }
          ],
          perQq: [
            { required: true, message: 'QQ号不能为空', trigger: 'blur' },
            {type: 'number', message: '请输入正确的QQ号码'}
          ],
          perSchoolPost: [
            { required: true, message: '在校职务不能为空', trigger: 'blur' }
          ],
          perSchoolHonor: [
            { required: true, message: '在校荣誉不能为空', trigger: 'blur' }
          ],
          perSocialPractice: [
            { required: true, message: '社会实践不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (item, index) {
        this.visible = true
        this.id = index || 0
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          this.id = index || 0
          if (this.id) {
            this.dataForm = JSON.parse(JSON.stringify(item))
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.visible = false
            this.$emit('refreshDataList', this.dataForm, this.id)
          }
        })
      }
    }
  }
</script>
