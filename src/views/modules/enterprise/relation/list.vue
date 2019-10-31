<template>
  <div class="mod-config">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-input v-model="dataForm.key" placeholder="参数名" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">查询</el-button>
        <el-button v-if="isAuth('enterprise:entpersoncooperationinfo:save')" type="primary" @click="addOrUpdateHandle()">新增</el-button>
        <el-button v-if="isAuth('enterprise:entpersoncooperationinfo:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量删除</el-button>
      </el-form-item>
    </el-form>
    <el-card v-if="!isAuth('enterprise:project:info:save')">
      <el-radio-group v-model="hasType" @change="getDataList">
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
        prop="proName"
        header-align="center"
        align="center"
        label="项目名称">
      </el-table-column>
      <el-table-column
        prop="sysUser.name"
        header-align="center"
        align="center"
        v-if="hasType == 'userPerId'"
        label="项目负责人">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="getStuDetailsInfo(scope.row.userPerId)">{{scope.row.sysUser.name}}</el-button>
        </template>
      </el-table-column>
      <el-table-column
        prop="sysUser.name"
        header-align="center"
        align="center"
        v-if="hasType == 'userTeacherId'"
        label="项目负责人">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="getTeaDetailsInfo(scope.row.userTeacherId)">{{scope.row.sysUser.name}}</el-button>
        </template>
      </el-table-column>
      <el-table-column
        prop="entEnterpriseInfo.entName"
        header-align="center"
        align="center"
        v-if="hasType == 'entInfoId'"
        label="发布企业">
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
          <el-button v-if="true" type="text" size="small" @click="detailHandle(scope.row.proInfoId)">合作列表</el-button>
          <el-button type="text" size="small" @click="deleteHandle(scope.row.proCooperationId)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="sizeChangeHandle"
      @current-change="currentChangeHandle"
      :current-page="pageIndex"
      :page-sizes="[10, 20, 50, 100]"
      :page-size="pageSize"
      :total="totalPage"
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
    <!-- 弹窗, 新增 / 修改 -->
    <relation-details v-if="shenhe" ref="details" @refreshDataList="getDetailsInfo()"/>
    <!-- 弹窗, 学生 / 教师 / 企业详情 -->
    <ent-details v-if="entDetails" ref="entDetails" @refreshDataList="getEntDetailsInfo()"/>
    <tea-details v-if="teaDetails" ref="teaDetails"/>
    <stu-details v-if="stuDetails" ref="stuDetails"/>
  </div>
</template>

<script>
import RelationDetails from './relation-details'
import EntDetails from '../base/ent-details'
import TeaDetails from '../base/tea-details'
import StuDetails from '../base/stu-details'
export default {
  data () {
    return {
      dataForm: {
        key: ''
      },
      shenhe: false,
      entDetails: false,
      teaDetails: false,
      stuDetails: false,
      dataList: [],
      pageIndex: 1,
      pageSize: 10,
      totalPage: 0,
      dataListLoading: false,
      dataListSelections: [],
      hasType: 'userPerId',
      inApply: '1',
      addOrUpdateVisible: false
    }
  },
  components: {
    RelationDetails,
    EntDetails,
    TeaDetails,
    StuDetails
  },
  activated () {
    this.getDataList()
  },
  methods: {
        // 获取数据列表
    getDataList () {
      this.dataListLoading = true
      this.$http({
        url: this.$http.adornUrl('/enterprise/person/cooperation/list'),
        method: 'get',
        params: this.$http.adornParams({
          'page': this.pageIndex,
          'limit': this.pageSize,
          'key': this.dataForm.key,
          'inType': this.hasType,
          'inApply': this.inApply
        })
      }).then(({data}) => {
        console.log(data)
        if (data && data.code === 0) {
          this.dataList = data.page.list
          this.totalPage = data.page.totalCount
        } else {
          this.dataList = []
          this.totalPage = 0
        }
        this.dataListLoading = false
      })
    },
      // 详情
    detailHandle (id) {
      console.log(id)
      this.shenhe = true
      this.$nextTick(() => {
        this.$refs.details.init(id, this.hasType)
      })
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
        // 每页数
    sizeChangeHandle (val) {
      this.pageSize = val
      this.pageIndex = 1
      this.getDataList()
    },
        // 当前页
    currentChangeHandle (val) {
      this.pageIndex = val
      this.getDataList()
    },
        // 多选
    selectionChangeHandle (val) {
      this.dataListSelections = val
    },
        // 新增 / 修改
    addOrUpdateHandle (id) {
      this.addOrUpdateVisible = true
      this.$nextTick(() => {
        this.$refs.addOrUpdate.init(id)
      })
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
