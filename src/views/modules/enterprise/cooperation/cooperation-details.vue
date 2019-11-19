<template>
  <el-dialog
    :title="'详情'"
    width="80%"
    v-loading="dataListLoading"
    @close="closeDialog"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-row>
      <table border="1" cellspacing="0" width="100%" class="table">
        <tr align='center'>
          <td colspan="10" style="height: 1.2rem"></td>
        </tr>
        <tr class="contents" align="center">
          <th colspan="10">
            项目基本信息
          </th>
        </tr>
        <tr align='center'>
          <td colspan="10" style="height: 1.2rem"></td>
        </tr>
        <tr align='center' style="height: 2.5rem">
          <th colspan="2">申报项目名称</th>
          <td colspan="3"><span style="font-size: 1.1rem; font-weight: bolder" v-text="project.proName" align="center"></span></td>
          <th colspan="2">项目类型</th>
          <td colspan="3" v-for="item in proTypeList"
              :key="item.value"
              v-if="item.value === project.proType">
            <span v-text="item.label" align="center"></span>
          </td>
        </tr>
        <tr align='center' style="height: 2.5rem">
          <th colspan="2">项目经费</th>
          <td colspan="3"><span  v-text="project.proOutlay" align="center"></span></td>
          <!--</tr>-->
          <!--<tr align='center' style="height: 2.5rem">-->
          <th colspan="2">项目来源</th>
          <td colspan="3"><span  v-text="project.proOrigin" align="center"></span></td>
          <!--</tr>-->
        <tr align='center'>
          <th colspan="2">项目介绍</th>
          <td colspan="8">
            <span v-text="project.proIntroduce" align="center"></span>
          </td>
        </tr>

        <tr align='center'>
          <td colspan="10" style="height: 1.2rem"></td>
        </tr>
        <tr class="contents" align="center">
          <th colspan="10">
            项目合作信息
          </th>
        </tr>
        <tr align='center'>
          <td colspan="10" style="height: 1.2rem"></td>
        </tr>
        <tr align='center'>
          <th colspan="2">合作内容</th>
          <td colspan="8">
            <span v-text="dataForm.cooperationContent" align="center"></span>
          </td>
        </tr>
        <tr align='center'>
          <th colspan="2">合作方式</th>
          <td colspan="8">
            <span v-text="dataForm.cooperationType" align="center"></span>
          </td>
        </tr>
        <tr align='center'>
          <th colspan="2">合作要求</th>
          <td colspan="8">
            <span v-text="dataForm.cooperationRequire" align="center"></span>
          </td>
        </tr>

      <template v-if="hasType === 'userPerId'">
        <!--项目负责人开始-->
        <tr align='center'>
          <td colspan="10" style="height: 1.2rem"></td>
        </tr>
        <tr class="contents"><th colspan="10">项目负责人信息</th></tr>
        <tr align='center'>
          <td colspan="10" style="height: 1.2rem"></td>
        </tr>
        <tr align='center'>
          <th>姓名</th>
          <td><span v-text="perInfo.sysUserEntity.name" align="center"></span></td>
          <th>性别</th>
          <template v-for="sex in sexList">
            <td v-if="perInfo.perSex === sex.value" v-text="sex.label"></td>
          </template>
          <th>企业职务</th>
          <td><span v-text="perInfo.perPost" align="center"></span></td>
          <th>联系电话</th>
          <td><span v-text="perInfo.sysUserEntity.mobile" align="center"></span></td>
          <th>QQ号码</th>
          <td><span v-text="perInfo.perQq" align="center"></span></td>
        </tr>

        <tr align='center'>
          <th>政治面貌</th>
          <td><span v-text="perInfo.perPoliticsType" align="center"></span></td>
          <th>学号</th>
          <td><span v-text="perInfo.perStuNo" align="center"></span></td>
          <th>所在二级学院</th>
          <td>
            <span v-for="item in instituteList"
                  v-if="item.instituteId === perInfo.sysUserEntity.instituteId"
                  v-text="item.instituteName"
                  align="center"></span>
          </td>
          <th>所在年级</th>
          <td>
            <span v-for="item in gradeList"
                  v-if="item.gradeId === perInfo.gradeId"
                  v-text="item.gradeName"
                  align="center">
            </span>
          </td>
          <th>所在班级</th>
          <td><span v-text="perInfo.perClassNo" align="center"></span></td>
        </tr>
        <tr align='center'>
          <th>所在宿舍</th>
          <td><span v-text="perInfo.perCormNo" align="center"></span></td>
          <th>个人电子邮箱</th>
          <td colspan="2"><span v-text="perInfo.sysUserEntity.email" align="center"></span></td>
          <th colspan="1">身份证号码</th>
          <td colspan="4"><span v-text="perInfo.perCardNo" align="center"></span></td>
        </tr>
        <tr align='center'>
          <th colspan="2">在校期间担任学生职务情况</th>
          <td colspan="8"><span v-text="perInfo.perSchoolPost" align="center"></span></td>
        </tr>
        <tr align='center'>
          <th colspan="2">在校期间所获等级证书及技能证书</th>
          <td colspan="8"><span v-text="perInfo.perSchoolHonor" align="center"></span></td>
        </tr>
        <tr align='center'>
          <th colspan="2">社会实践主要成绩简述</th>
          <td colspan="8"><span v-text="perInfo.perSocialPractice" align="center"></span></td>
        </tr>
        <!--评分结束-->
      </template>

      <template v-if="entProjectAttachEntities.length>0">
        <tr align='center'>
          <td colspan="10" style="height: 1.2rem"></td>
        </tr>
        <tr align="center" class="contents">
          <th colspan="10">项目信息附件</th>
        </tr>
        <tr align='center'>
          <td colspan="10" style="height: 1.2rem"></td>
        </tr>
        <tr align='center'>
          <th colspan="7">附件名</th>
          <th colspan="3">操作</th>
        </tr>
        <tr align='center'>
          <td colspan="10" style="height: 1.2rem"></td>
        </tr>
        <template>
          <tr v-for="item in entProjectAttachEntities"
              align="center">
            <td colspan="7" v-text="item.attachName"></td>
            <td colspan="3"><el-button @click="attachDown(item)" :loading="downloadLoading">下载</el-button></td>
          </tr>
        </template>

        <!-- 合作附件开始 -->
        <template v-if="entProjectAttachEntities.length>0">
          <tr align='center'>
            <td colspan="10" style="height: 1.2rem"></td>
          </tr>
          <tr align="center" class="contents">
            <th colspan="10">项目合作附件</th>
          </tr>
          <tr align='center'>
            <td colspan="10" style="height: 1.2rem"></td>
          </tr>
          <tr align='center'>
            <th colspan="7">附件名</th>
            <th colspan="3">操作</th>
          </tr>
          <tr align='center'>
            <td colspan="10" style="height: 1.2rem"></td>
          </tr>
          <template>
            <tr v-for="item in entProjectAttachEntities"
                align="center">
              <td colspan="7" v-text="item.attachName"></td>
              <td colspan="3"><el-button @click="attachDown(item)" :loading="downloadLoading">下载</el-button></td>
            </tr>
          </template>
        <tr align='center'>
          <td colspan="10" style="height: 1.2rem"></td>
        </tr>
      </template>
      </template>
      </table>
    </el-row>
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
      dataListLoading: false,
      downloadLoading: false,
      hasType: 'userPerId',
      perInfo: {
        sysUserEntity: ''
      },
      Cooperation: '',
      instituteList: this.$store.state.user.institute,
      gradeList: this.$store.state.user.grade,
      entProjectAttachEntities: [],
      sexList: [
        {value: 1, label: '男'}, {value: 2, label: '女'}
      ],
      proTypeList: [
        {value: 1, label: '科研项目'},
        {value: 2, label: '横向项目'},
        {value: 3, label: '企业项目'},
        {value: 4, label: '大创项目'},
        {value: 5, label: '企业招聘'},
        {value: 6, label: '实习项目对接'}
      ],
      project: '',
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
              this.perInfo = data.data.userPersonInfo
              this.perInfo.sysUserEntity = data.data.sysUserEntity
              this.entProjectAttachEntities = data.data.entProjectAttachEntities
              this.project = data.data.projectInfo
            }
          })
        }
      })
    },
    closeDialog () {
      this.visible = false
      this.$emit('refreshDataList')
    },
    attachDown (attach) {
      this.downloadLoading = true
      this.$httpFile({
        url: this.$httpFile.adornUrl(`/enterprise/project/attach/download`),
        method: 'post',
        params: this.$httpFile.adornParams({
          'filePath': attach.url
        })
      }).then(response => {
        if (!response) {
          this.downloadLoading = false
          return
        }
        let url = window.URL.createObjectURL(new Blob([response.data]))
        let link = document.createElement('a')
        link.style.display = 'none'
        link.href = url
        link.setAttribute('download', attach.attachName)
        document.body.appendChild(link)
        link.click()
        this.downloadLoading = false
      }).catch(err => {
        console.log(err)
        this.downloadLoading = false
      })
    }
  }
}
</script>
<style>
  .contents{
    height: 60px;
    font-size: 18px;
  }
  .el-card__body {
    padding: 10px;
  }
  .el-step__title {
    font-size: 12px;
    line-height: 28px;
  }
  .table {
    border: 1px solid #ddd;
    /*为表格设置合并边框模型*/
    border-collapse: collapse;
    /*列宽由表格宽度和列宽度设定*/
    table-layout: fixed;
  }
  .table td {
    /*允许在单词内换行。*/
    word-break: break-word;
    /*设置宽度*/
    width: 80%;
  }
  .table th {
    /*允许在单词内换行。*/
    word-break: break-word;
    /*设置宽度*/
    width: 80%;
  }
</style>
