<template>
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
        label="申请者">
<!--        <template slot-scope="scope">-->
<!--          <el-button type="text" plain size="small" @click="details(scope.row.userPersonInfo.sysUserEntity.name)">{{scope.row.userPersonInfo}}</el-button>-->
<!--        </template>-->
      </el-table-column>
      <el-table-column
        prop="userTeacherInfo.sysUserEntity.name"
        header-align="center"
        align="center"
        v-if="hasType == 'userTeacherId'"
        label="申请者">
        <!--        <template slot-scope="scope">-->
        <!--          <el-button type="text" plain size="small" @click="details(scope.row.userPersonInfo.sysUserEntity.name)">{{scope.row.userPersonInfo}}</el-button>-->
        <!--        </template>-->
      </el-table-column>
      <el-table-column
        prop="entEnterpriseInfo.entName"
        header-align="center"
        align="center"
        v-if="hasType == 'entInfoId'"
        label="申请企业">
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
          <el-button v-if="true" type="text" size="small"  @click="deleteHandle(scope.row.proCooperationInfoId)">删除</el-button>
          <el-button v-else type="text" size="small">无操作</el-button>
        </template>
      </el-table-column>
    </el-table>
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
      console.log(this.dataList)
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
}
</script>
