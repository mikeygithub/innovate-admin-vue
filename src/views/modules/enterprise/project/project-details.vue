<template>
  <div class="mod-user">
  <el-dialog
    :title="'项目信息详情'"
    width="60%"
    :modal-append-to-body='false'
    :visible.sync="visible">
    <el-row v-if="!isAuth('enterprise:project:info:save')" :gutter="20">
      <el-col :offset="20"><el-button type="primary" v-if="dataForm.userPerId" @click="getStuDetailsInfo(dataForm.userPerId)">负责人详情</el-button></el-col>
    </el-row>
    <el-row v-if="!isAuth('enterprise:project:info:save')">
      <el-button type="primary" v-if="dataForm.userTeacherId" @click="getTeaDetailsInfo(dataForm.userTeacherId)">负责人详情</el-button>
    </el-row>
    <el-row v-if="!isAuth('enterprise:project:info:save')">
      <el-button type="primary" v-if="dataForm.entInfoId" @click="getEntDetailsInfo(dataForm.entInfoId)">企业详情</el-button>
    </el-row>
    <div class="d1"></div>
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

    <span slot="footer" class="dialog-footer">
      <el-button type="primary" @click="visible = false">返回</el-button>
    </span>
    <!-- 弹窗, 学生 / 教师 / 企业详情 -->
    <ent-details v-if="entDetails" ref="entDetails" @refreshDataList="getEntDetailsInfo()"/>
    <tea-details v-if="teaDetails" ref="teaDetails"/>
    <stu-details v-if="stuDetails" ref="stuDetails"/>
  </el-dialog>
  </div>
</template>

<script>
import EntDetails from '../base/ent-details'
import TeaDetails from '../base/tea-details'
import StuDetails from '../base/stu-details'
export default {
  data () {
    return {
      visible: false,
      entDetails: false,
      teaDetails: false,
      stuDetails: false,
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
  components: {
    EntDetails,
    TeaDetails,
    StuDetails
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
      // 企业详情弹窗
    getEntDetailsInfo (id, hasApply) {
      console.log(id + hasApply)
      this.entDetails = true
      this.$nextTick(() => {
        this.$refs.entDetails.init(id, hasApply)
      })
    },
      // 教师详情弹窗
    getTeaDetailsInfo (id) {
      console.log(id)
      this.teaDetails = true
      this.$nextTick(() => {
        this.$refs.teaDetails.init(id)
      })
    },
      // 学生详情弹窗
    getStuDetailsInfo (id) {
      console.log(id)
      this.stuDetails = true
      this.$nextTick(() => {
        this.$refs.stuDetails.init(id)
      })
    },
    handleChange (val) {
      console.log(val)
    }
  }
}
</script>

<style>
.d1{
  padding-bottom: 15px;
}
</style>
