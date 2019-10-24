<template>
  <div class="mod-user">
  <el-dialog
    :title="'申请合作列表'"
    :close-on-click-modal="false"
    width="75%"
    :visible.sync="visible">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="init()">
      <el-form-item>
        <el-input v-model="dataForm.key" placeholder="参数名" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="init()">查询</el-button>
        <el-button type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量删除</el-button>
      </el-form-item>
    </el-form>
    <el-card>
      <el-radio-group v-model="hasType" @change="changeType(hasType)">
        <el-radio label="userPerId">学生</el-radio>
        <el-radio label="userTeacherId">教师</el-radio>
        <el-radio label="entInfoId">企业</el-radio>
      </el-radio-group>
    </el-card>
    <el-table
      :data="dataList"
      border
      v-loading="dataListLoading"
      @selection-change="selectionChangeHandle"
      style="width: 100%;">
      <el-table-column
        type="selection"
        header-align="center"
        align="center"
        width="50">
      </el-table-column>
      <el-table-column
        prop="userPersonInfo.sysUserEntity.name"
        header-align="center"
        align="center"
        v-if="hasType == 'userPerId'"
        label="项目负责人">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="getStuDetailsInfo(scope.row.userPersonInfo.userPerId)">{{scope.row.userPersonInfo.sysUserEntity.name}}</el-button>
        </template>
      </el-table-column>
      <el-table-column
        prop="userTeacherInfo.sysUserEntity.name"
        header-align="center"
        align="center"
        v-if="hasType == 'userTeacherId'"
        label="项目负责人">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="getTeaDetailsInfo(scope.row.userTeacherInfo.userTeacherId)">{{scope.row.userTeacherInfo.sysUserEntity.name}}</el-button>
        </template>
      </el-table-column>
      <el-table-column
        prop="entEnterpriseInfo.entName"
        header-align="center"
        align="center"
        v-if="hasType == 'entInfoId'"
        label="申请企业">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="getEntDetailsInfo(scope.row.entEnterpriseInfo.entInfoId, scope.row.entEnterpriseInfo.inApply)">{{scope.row.entEnterpriseInfo.entName}}</el-button>
        </template>
      </el-table-column>
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="150"
        label="操作">
        <template slot-scope="scope">
          <!-- isAuth('enterprise:info:shenhe') -->
          <el-button v-if="true" type="text" size="small" @click="detailHandle(scope.row.proCooperationInfoId)">详情</el-button>
          <el-button v-if="true" type="text" size="small"  @click="deleteHandle(scope.row.proCooperationInfoId)">移除</el-button>
          <el-button v-else type="text" size="small">无操作</el-button>
        </template>
      </el-table-column>
    </el-table>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
    <el-pagination
      @size-change="sizeChangeHandle"
      @current-change="currentChangeHandle"
      :current-page="pageIndex"
      :page-sizes="[5, 10, 20, 50, 100]"
      :page-size="pageSize"
      :total="totalPage"
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
    <!-- 弹窗, 学生 / 教师 / 企业详情 -->
    <ent-details v-if="entDetails" ref="entDetails" @refreshDataList="getEntDetailsInfo()"/>
    <tea-details v-if="teaDetails" ref="teaDetails"/>
    <stu-details v-if="stuDetails" ref="stuDetails"/>
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
      proInfoId: '',
      applyName: '',
      dataList: [],
      dataStuList: [],
      dataTeaList: [],
      dataEntList: [],
      dataListSelections: [],
      dataForm: {
        key: '',
        proName: '',
        proCooperationInfoId: 0,
        cooperationContent: '',
        cooperationType: '',
        cooperationRequire: '',
        userPerId: '',
        userTeacherId: '',
        entInfoId: '',
        inApply: ''
      },
      pageIndex: 1,
      pageSize: 10,
      totalPage: 0,
      dataListLoading: false,
      hasType: 'userPerId'
    }
  },
  components: {
    EntDetails,
    TeaDetails,
    StuDetails
  },
  methods: {
    init (id, hasType) {
      this.proInfoId = id || 0
      this.visible = true
      this.hasType = hasType
      console.log(this.proInfoId)
      console.log(this.hasType)
      this.$nextTick(() => {
        if (this.proInfoId) {
          this.$http({
            url: this.$http.adornUrl(`/enterprise/person/cooperation/info/${this.proInfoId}/${this.hasType}`),
            params: this.$http.adornParams({
              'page': this.pageIndex,
              'limit': this.pageSize,
              'key': this.dataForm.key,
              'inType': this.hasType
            })
          }).then(({data}) => {
            console.log(data)
            if (data && data.code === 0) {
              this.dataList = data.data.projectCooperationInfo.personCooperationPer
              this.dataStuList = data.data.projectCooperationInfo.personCooperationPer
              this.dataTeaList = data.data.projectCooperationInfo.personCooperationTeacher
              this.dataEntList = data.data.projectCooperationInfo.personCooperationEnt
              this.totalPage = data.page.totalCount
            } else {
              this.dataList = []
              this.totalPage = 0
            }
            this.dataListLoading = false
            console.log(this.dataList)
          })
        }
      })
    },
      // 获取数据列表
    getDataList () {
      this.dataListLoading = true
      console.log(this.proInfoId + this.hasType)
      this.$http({
        url: this.$http.adornUrl(`/enterprise/person/cooperation/info/${this.proInfoId}/${this.hasType}`),
        method: 'get',
        params: this.$http.adornParams({
          'page': this.pageIndex,
          'limit': this.pageSize,
          'key': this.dataForm.key,
          'inType': this.hasType
        })
      }).then(({data}) => {
        if (data && data.code === 0) {
          if (this.hasType === 'userPerId') {
            this.dataList = data.data.projectCooperationInfo.personCooperationPer
          }
          if (this.hasType === 'userTeacherId') {
            this.dataList = data.data.projectCooperationInfo.personCooperationTeacher
          }
          if (this.hasType === 'entInfoId') {
            this.dataList = data.data.projectCooperationInfo.personCooperationEnt
          }
          this.totalPage = data.page.totalCount
        } else {
          this.dataList = []
          this.totalPage = 0
        }
        this.dataListLoading = false
      })
    },
    changeType (hasType) {
      if (hasType === 'userPerId') {
        this.dataList = this.dataStuList
      }
      if (hasType === 'userTeacherId') {
        this.dataList = this.dataTeaList
      }
      if (hasType === 'entInfoId') {
        this.dataList = this.dataEntList
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
      // 教师详情弹窗
    getStuDetailsInfo (id) {
      console.log(id)
      this.stuDetails = true
      this.$nextTick(() => {
        this.$refs.stuDetails.init(id)
      })
    }
  },
  details (id) {
    console.log(id)
  },
      // 删除
  deleteHandle (id) {
    var ids = id ? [id] : this.dataListSelections.map(item => {
      return item.proCooperationId
    })
    this.$confirm(`确定对[id=${ids.join(',')}]进行[${id ? '删除' : '批量删除'}]操作?`, '提示', {
      confirmButtonText: '确定',
      cancelButtonText: '取消',
      type: 'warning'
    }).then(() => {
      this.$http({
        url: this.$http.adornUrl('/enterprise/entpersoncooperationinfo/delete'),
        method: 'post',
        data: this.$http.adornData(ids, false)
      }).then(({data}) => {
        if (data && data.code === 0) {
          this.$message({
            message: '操作成功',
            type: 'success',
            duration: 1500,
            onClose: () => {
              this.getDataList()
            }
          })
        } else {
          this.$message.error(data.msg)
        }
      })
    })
  }
}
</script>
