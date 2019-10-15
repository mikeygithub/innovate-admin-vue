<template>
  <el-dialog
    :title="'项目信息详情'"
    :close-on-click-modal="false"
    width="60%"
    :visible.sync="visible">
    <el-form :model="dataForm" ref="dataForm" label-width="150px">
      <el-form-item label="项目名称" prop="proName">
        <el-input v-model="dataForm.proName" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="项目登记" prop="proRegister">
        <el-input v-model="dataForm.proRegister" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="项目来源" prop="proOrigin">
        <el-input type="textarea" v-model="dataForm.proOrigin" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="项目经费" prop="proOutlay">
        <el-input v-model="dataForm.proOutlay" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="项目类型" prop="proType">
        <el-input v-model="dataForm.proType" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="项目介绍" prop="proIntroduce">
        <el-input type="textarea" v-model="dataForm.proIntroduce" :readonly="true"></el-input>
      </el-form-item>
    </el-form>
    <el-collapse v-if="dataForm.entInfoId" v-model="activeNames" @change="handleChange">
      <el-collapse-item title="企业详情" name="1">
        <div>
          <el-form :model="dataForm" ref="dataForm" label-width="150px">
            <el-form-item label="发布者姓名" prop="name">
              <el-input v-model="dataForm.userTeacherInfo.SysUserEntity.name" :readonly="true"></el-input>
            </el-form-item>
            <el-form-item label="教师职务" prop="teacherPost">
              <el-input v-model="dataForm.userTeacherInfo.teacherPost" :readonly="true"></el-input>
            </el-form-item>

          </el-form>
        </div>
      </el-collapse-item>
    </el-collapse>

    <el-collapse v-if="dataForm.userPerId" v-model="activeNames" @change="handleChange">
      <el-collapse-item title="发布者详情" name="2">
        <div>
          <el-form :model="dataForm" ref="dataForm" label-width="150px">
            <el-form-item label="发布者姓名" prop="name">
            <el-input v-model="dataForm.userPersonInfo.SysUserEntity.name" :readonly="true"></el-input>
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
        </div>
      </el-collapse-item>
    </el-collapse>

    <el-collapse v-if="dataForm.userTeacherId" v-model="activeNames" content="true" @change="handleChange">
      <el-collapse-item title="发布者详情" name="3">
        <div>
          <el-form :model="dataForm" ref="dataForm" label-width="150px">
            <el-form-item label="发布者姓名" prop="name">
              <el-input v-model="dataForm.userTeacherInfo.SysUserEntity.name" :readonly="true"></el-input>
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
        </div>
      </el-collapse-item>
    </el-collapse>

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
        activeNames: ['1'],
        hasType: 'userPerId',
        dataForm: {
          proInfoId: 0,
          proName: '',
          proRegister: '',
          proOrigin: '',
          proOutlay: '',
          proType: '',
          proIntroduce: '',
          inApply: ''
        }
      }
    },
    methods: {
      init (hasType, id) {
        this.visible = true
        this.dataForm.proInfoId = id
        this.hasType = hasType
        console.log(`${this.hasType}/${this.dataForm.proInfoId}`)
        if (this.dataForm.proInfoId) {
          this.$http({
            url: this.$http.adornUrl(`/enterprise/project/info/info/${this.hasType}/${this.dataForm.proInfoId}`),
            method: 'get',
            params: this.$http.adornParams()
          }).then(({data}) => {
            console.log(data)
            if (data && data.code === 0) {
              this.dataForm = data.data
            }
          })
        }
      },
      handleChange (val) {
        console.log(val)
      }
    }
  }
</script>
