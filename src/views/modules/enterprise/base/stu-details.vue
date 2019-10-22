<template>
  <el-dialog
    :title="'学生信息详情'"
    :close-on-click-modal="false"
    width="60%"
    :visible.sync="visible">
    <el-form :model="dataForm" ref="dataForm" label-width="150px">
      <el-form-item label="发布者姓名" prop="name">
        <el-input v-model="dataForm.sysUser.name" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="政治面貌" prop="perPoliticsType">
        <el-input v-model="dataForm.userPersonInfo.perPoliticsType" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="学号" prop="perStuNo">
        <el-input v-model="dataForm.userPersonInfo.perStuNo" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="籍贯" prop="perNative">
        <el-input v-model="dataForm.userPersonInfo.perNative" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="年级" prop="gradeId">
        <el-input v-model="dataForm.userPersonInfo.gradeId" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="班级" prop="perClassNo">
        <el-input v-model="dataForm.userPersonInfo.perClassNo" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="宿舍" prop="perCormNo">
        <el-input v-model="dataForm.userPersonInfo.perCormNo" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="身份证号" prop="perCardNo">
        <el-input v-model="dataForm.userPersonInfo.perCardNo" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="性别" prop="perSex">
        <el-input v-model="dataForm.userPersonInfo.perSex" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="在校职务" prop="perSchoolPost">
        <el-input v-model="dataForm.userPersonInfo.perSchoolPost" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="兴趣爱好" prop="perInterest">
        <el-input v-model="dataForm.userPersonInfo.perInterest" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="年龄" prop="perAge">
        <el-input v-model="dataForm.userPersonInfo.perAge" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="所获荣誉" prop="perSchoolHonor">
        <el-input v-model="dataForm.userPersonInfo.perSchoolHonor" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="社会实践" prop="perSocialPractice">
        <el-input v-model="dataForm.userPersonInfo.perSocialPractice" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="工作方式" prop="perWorking">
        <el-input v-model="dataForm.userPersonInfo.perWorking" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="企业职务" prop="perPost">
        <el-input v-model="dataForm.userPersonInfo.perPost" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="QQ" prop="perQq">
        <el-input v-model="dataForm.userPersonInfo.perQq" :readonly="true"></el-input>
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
