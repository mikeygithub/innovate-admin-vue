<template>
  <div class="mod-user">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-select v-model="instituteId" placeholder="请选择二级学院">
          <el-option
            v-for="inst in instituteList"
            :key="inst.instituteName"
            :label="inst.instituteName"
            :value="inst.instituteId">
          </el-option>
        </el-select>
        <!--<el-button v-if="isAuth('sys:user:save')" type="primary" @click="addOrUpdateHandle()">新增</el-button>-->
        <!--<el-button v-if="isAuth('sys:user:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量删除</el-button>-->
        <!--年度 start-->
          <el-select v-model="declareYear" placeholder="请选择年度">
            <el-option
              v-for="item in declareYears"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        <!--年度 end-->
        <el-button @click="getDataList()">查询</el-button>
      </el-form-item>
    </el-form>
    <el-table
      :data="dataList"
      border
      v-loading="dataListLoading"
      style="width: 100%;">
      <el-table-column
        type="selection"
        header-align="center"
        align="center"
        width="50">
      </el-table-column>
      <el-table-column
        prop="instituteId"
        header-align="center"
        align="center"
        width="80"
        label="ID">
      </el-table-column>
      <el-table-column
        prop="instituteName"
        header-align="center"
        align="center"
        label="二级学院">
      </el-table-column>
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="150"
        label="操作">
        <template slot-scope="scope">
          <!--<el-button v-if="isAuth('sys:user:update')" type="text" size="small" @click="addOrUpdateHandle(scope.row.instituteId)">修改</el-button>-->
          <!--<el-button v-if="isAuth('sys:user:delete')" type="text" size="small" @click="deleteHandle(scope.row.instituteId)">删除</el-button>-->
          <el-button  type="text" size="small" @click="collectDetail(scope.row.instituteId)">查看</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!--<el-pagination-->
      <!--@size-change="sizeChangeHandle"-->
      <!--@current-change="currentChangeHandle"-->
      <!--:current-page="pageIndex"-->
      <!--:page-sizes="[10, 20, 50, 100]"-->
      <!--:page-size="pageSize"-->
      <!--:total="totalPage"-->
      <!--layout="total, sizes, prev, pager, next, jumper">-->
    <!--</el-pagination>-->
    <!-- 弹窗, 新增 / 修改 -->
    <self-er-match-collect-detail v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></self-er-match-collect-detail>
  </div>
</template>

<script>
  import SelfErMatchCollectDetail from './operation/self-er-match-collect-detail'
  export default {
    data () {
      return {
        dataForm: {
          userName: '',
          instituteId: '',
          declareYear: '',
          institute: {}
        },
        instituteId: '',
        declareYear: '',
        declareYears: [],
        instituteList: this.$store.state.user.institute,
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: [],
        addOrUpdateVisible: false
      }
    },
    components: {
      SelfErMatchCollectDetail
    },
    activated () {
      this.getDataList()
    },
    methods: {
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/sys/user/info/' + this.$store.state.user.id),
          method: 'get'
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.$http({
              url: this.$http.adornUrl('/innovate/sys/institute/info/' + data.user.instituteId),
              method: 'get'
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataList = []
                this.dataList.push(data.institute)
                this.dataListLoading = false
              }
            })
          } else {
            this.$message.error(data.msg)
          }
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
      collectDetail (id) {
        this.addOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs.addOrUpdate.init(id)
        })
      }
      // // 新增 / 修改
      // collectDetail (id) {
      //   this.addOrUpdateVisible = true
      //   this.$nextTick(() => {
      //     this.$refs.addOrUpdate.init(id)
      //   })
      // }
      // ,
      // 删除
      // deleteHandle (id) {
      //   var instituteIds = id ? [id] : this.dataListSelections.map(item => {
      //     return item.instituteId
      //   })
      //   this.$confirm(`确定对[id=${instituteIds.join(',')}]进行[${id ? '删除' : '批量删除'}]操作?`, '提示', {
      //     confirmButtonText: '确定',
      //     cancelButtonText: '取消',
      //     type: 'warning'
      //   }).then(() => {
      //     this.$http({
      //       url: this.$http.adornUrl('/innovate/sys/institute/delete'),
      //       method: 'post',
      //       data: this.$http.adornData(instituteIds, false)
      //     }).then(({data}) => {
      //       if (data && data.code === 0) {
      //         this.$message({
      //           message: '操作成功',
      //           type: 'success',
      //           duration: 1500,
      //           onClose: () => {
      //             this.getDataList()
      //           }
      //         })
      //       } else {
      //         this.$message.error(data.msg)
      //       }
      //     })
      //   }).catch(() => {})
      // }
    }
  }
</script>
