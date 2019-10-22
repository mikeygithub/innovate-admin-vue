<template>
  <el-dialog
    :title="'企业审核'"
    :close-on-click-modal="false"
    width="60%"
    :visible.sync="visible">
    <el-form :model="dataForm" ref="dataForm" label-width="150px">
      <el-form-item label="发布者姓名" prop="name">
        <el-input v-model="dataForm.sysUser.name" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="教师职务" prop="teacherPost">
        <el-input v-model="dataForm.userTeacherInfo.teacherPost" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="身份证号" prop="teacherCardNo">
        <el-input v-model="dataForm.userTeacherInfo.teacherCardNo" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="性别" prop="teacherSex">
        <el-input v-model="dataForm.userTeacherInfo.teacherSex" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="教师职称" prop="teacherTitle">
        <el-input v-model="dataForm.userTeacherInfo.teacherTitle" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="教师学历" prop="teacherBackground">
        <el-input v-model="dataForm.userTeacherInfo.teacherBackground" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="教师学位" prop="teacherDegree">
        <el-input v-model="dataForm.userTeacherInfo.teacherDegree" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="所学方向" prop="teacherStudy">
        <el-input v-model="dataForm.userTeacherInfo.teacherStudy" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="从事科研领域" prop="teacherScientific">
        <el-input v-model="dataForm.userTeacherInfo.teacherScientific" :readonly="true"></el-input>
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
        dataList: [],
        dataForm: {
        }
      }
    },
    methods: {
      init (id, hasApply) {
        this.visible = true
        this.dataForm.entInfoId = id || 0
        this.dataForm.inApply = hasApply || 1
        console.log(id + hasApply)
        if (this.dataForm.entInfoId) {
          this.$http({
            url: this.$http.adornUrl(`/enterprise/info/info/${this.dataForm.entInfoId}/${this.dataForm.inApply}`),
            method: 'get',
            params: this.$http.adornParams()
          }).then(({data}) => {
            console.log(this.dataForm.entInfoId)
            console.log(this.dataForm.inApply)
            if (data && data.code === 0) {
              this.dataForm = data.data
            }
          })
        }
      }
    }
  }
</script>
