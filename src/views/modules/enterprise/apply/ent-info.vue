<template>
  <div class="mod-user">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-date-picker
          v-model="dataForm.matchTime"
          align="right"
          type="year"
          placeholder="请选择年度">
        </el-date-picker>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.projectName" placeholder="项目名" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">查询</el-button>
      </el-form-item>
    </el-form>
    <el-card>
      <el-radio-group v-model="hasApply" @change="getDataList">
        <el-radio label="0">未审批</el-radio>
        <el-radio label="1">已审批</el-radio>
      </el-radio-group>
    </el-card>
    <el-table
      :data="dataList"
      border
      v-loading="dataListLoading"
      :default-sort="{prop: 'entInfoId', order: 'ascending'}"
      @selection-change="selectionChangeHandle"
      style="width: 100%;">
      <el-table-column
        type="selection"
        header-align="center"
        align="center"
        width="50">
      </el-table-column>
      <!--      <el-table-column-->
      <!--        hidden-->
      <!--        header-align="center"-->
      <!--        align="center"-->
      <!--        width="50"-->
      <!--        label="ID">-->
      <!--        <template slot-scope="props">-->
      <!--          <p v-text="props.$index+1"></p>-->
      <!--        </template>-->
      <!--      </el-table-column>-->
      <!--      <el-table-column-->
      <!--        sortable-->
      <!--        hidden-->
      <!--        type="expand"-->
      <!--        prop="matchInfoEntity.matchId"-->
      <!--        header-align="center"-->
      <!--        align="center"-->
      <!--        width="120"-->
      <!--        label="展开流程进度">-->
      <!--        <template slot-scope="props">-->
      <!--          <el-row>-->
      <!--            <el-card style=": 0.1rem">-->
      <!--              <el-col :span="3">-->
      <!--                <el-tag>比赛申请审批进度</el-tag>-->
      <!--              </el-col>-->
      <!--              <el-col :span="21">-->
      <!--                <el-steps-->
      <!--                  :active="props.row.matchInfoEntity.projectMatchApplyStatus"-->
      <!--                  finish-status="success">-->
      <!--                  <el-step title="项目负责人提交"></el-step>-->
      <!--                  <el-step title="指导老师审批"></el-step>-->
      <!--                  <el-step title="二级学院审批"></el-step>-->
      <!--                  <el-step title="管理员分配评委组"></el-step>-->
      <!--                  <el-step title="评委审批"></el-step>-->
      <!--                  <el-step title="管理员审批"></el-step>-->
      <!--                  &lt;!&ndash;<el-step title="超级管理员审批"></el-step>&ndash;&gt;-->
      <!--                </el-steps>-->
      <!--              </el-col>-->
      <!--            </el-card>-->
      <!--          </el-row>-->
      <!--        </template>-->
      <!--      </el-table-column>-->
      <el-table-column
        sortable
        prop="entName"
        header-align="center"
        align="center"
        label="企业名称">
      </el-table-column>
      <el-table-column
        sortable
        prop="entCorporate"
        header-align="center"
        align="center"
        width="110"
        label="法人代表">
      </el-table-column>
      <el-table-column
        sortable
        prop="entCode"
        header-align="center"
        align="center"
        label="统一信用代码">
      </el-table-column>
      <el-table-column
        sortable
        prop="entType"
        header-align="center"
        align="center"
        label="企业类型">
      </el-table-column>
      <el-table-column
        sortable
        prop="newHighZones"
        header-align="center"
        align="center"
        width="120"
        :formatter="formatterZone"
        label="是否高新区">
      </el-table-column>
      <el-table-column
        sortable
        prop="entInTime"
        header-align="center"
        align="center"
        label="申请入驻时间">
      </el-table-column>
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="110"
        label="操作">
        <template slot-scope="scope">
          <!-- isAuth('enterprise:info:shenhe') -->
          <el-button v-if="true" type="text" size="small" @click="applyDetailHandle(scope.row.entInfoId)">审核</el-button>
          <el-button v-else type="text" size="small" >无操作</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="sizeChangeHandle"
      @current-change="currentChangeHandle"
      :current-page="pageIndex"
      :page-sizes="[5, 10, 20, 50, 100]"
      :page-size="pageSize"
      :total="totalPage"
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
    <!-- 弹窗, 新增 / 修改 -->
  </div>
</template>

<script>
  export default {
    data () {
      return {
        hasApply: '0',
        dataForm: {
          baseId: '',
          projectName: '',
          matchTime: new Date(),
          idDel: 0
        },
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: []
      }
    },
    components: {
    },
    activated () {
      this.getDataList()
    },
    methods: {
      // 格式化区域显示
      formatterZone (row, column, cellValue) {
        return cellValue === '0' ? '是' : '否'
      },
      // 审核
      applyDetailHandle (id) {
        console.log(id)
      },
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/enterprise/info/list'),
          method: 'get',
          params: this.$http.adornParams({
            'currPage': this.pageIndex,
            'pageSize': this.pageSize,
            'inApply': this.hasApply
          })
        }).then(({data}) => {
          console.log(data)
          if (data.code === 500) {
            this.$message.error(data.msg)
          }
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
      //  每页数
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
      // 审批
      applyMatchHandle (id) {
        this.$nextTick(() => {
          // this.$refs.award.init(id)
        })
      },
      // 审批
      applyMatchRef (id) {
        this.$http({
          url: this.$http.adornUrl('/innovate/match/apply/apply'),
          method: 'post',
          params: this.$http.adornParams({
            'matchId': id,
            'apply': 'project_match_apply_status',
            'roleId': 5
          }, false)
        }).then(({data}) => {
          this.$message({
            type: 'success',
            message: '提交成功!'
          })
          this.getDataList()
        })
      },
      isDelete (item) {
        if ((item.projectMatchApplyStatus === 0) &&
          this.isAuth('innovate:match:delete')) {
          return true
        }
      },
      // 删除
      deleteHandle (id) {
        var canDelete = true
        var matchIds = id ? [id] : this.dataListSelections.map(item => {
          if ((item.projectInfoEntity.projectMatchApplyStatus > 0) ||
            !this.isAuth('innovate:match:delete')) {
            canDelete = false
          } else {
            canDelete = false
          }
          return item.projectInfoEntity.matchId
        })
        this.$confirm(`确定要进行[${id ? '删除' : '批量删除'}]操作?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          if (canDelete) {
            this.$http({
              url: this.$http.adornUrl('/innovate/match/info/delete'),
              method: 'post',
              data: this.$http.adornData(matchIds, false)
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
          } else {
            this.$message({
              message: '包含不可删除项目',
              type: 'error',
              duration: 1500,
              onClose: () => {
                this.getDataList()
              }
            })
          }
        }).catch(() => {})
      }
    }
  }
</script>
<style>
  .el-step__title {
    font-size: 12px;
    line-height: 28px;
  }
  .el-table__expanded-cell[class*=cell] {
    padding: 5px 5px;
  }
</style>
