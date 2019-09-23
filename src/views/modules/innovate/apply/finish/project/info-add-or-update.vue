<template>
  <el-dialog
    width="60%"
    @close="closeDialog"
    v-loading="dataListLoading"
    :title="!dataForm.id ? '新增结题信息' : '修改结题信息'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-row :gutter="20">
      <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="16rem" style="width: 94%; margin: 0 auto">
        <el-col :span="24">
          <el-form-item label="申报名称" prop="finishName">
            <el-input v-model="dataForm.finishName" placeholder="请输入申报名称"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="24">
          <el-form-item label="申报类型" prop="finishType">
            <el-select v-model="dataForm.finishType"  placeholder="请选择">
              <el-option v-for="item in finishTypeList" :key="item.value" :label="item.label" :value="item.value">
              </el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="24">
          <el-form-item label="指导老师" prop="teacherLists">
            <el-button size="mini"
                       v-if="addVisible(teacherLists)"
                       type="primary" plain @click="addTeacher()">添加</el-button>
            <template v-for="(item,index) in teacherLists" v-if="item.isDel !== 1">
              <el-col :span="24">
                <el-tag style="margin-right: 1rem"
                        v-for="user in userTeacherInfoEntities"
                        :key="user.userId"
                        v-if="user.userId === item.userId"
                        v-text="'姓名：' + user.name">
                </el-tag>
                <el-button size="mini" type="primary" plain @click="addTeacher()">添加</el-button>
                <el-button size="mini" type="primary" plain @click="addTeacher(item, index)">修改</el-button>
                <el-button size="mini" type="danger" @click="delTeacher(item, index)">删除</el-button>
              </el-col>
            </template>
            <span v-if="isTeacherInfoNullVisible" style="color: crimson">*请联系指导老师完善个人信息</span>
          </el-form-item>
        </el-col>
        <el-col :span="24">
          <el-form-item label="项目参与者信息" prop="staffInfoLists">
            <el-button size="mini"
                       v-if="addVisible(staffInfoLists)" type="primary" plain @click="addStaff()">添加</el-button>
            <template v-for="(item,index) in staffInfoLists" v-if="item.isDel !== 1">
              <el-col :span="24">
                <el-tag style="margin-right: 1rem"
                        v-text="item.staffName">
                </el-tag>
                <el-button size="mini" type="primary" plain @click="addStaff()">添加</el-button>
                <el-button size="mini" type="primary" plain @click="addStaff(item, index)">修改</el-button>
                <el-button size="mini" type="danger" @click="delStaff(item, index)">删除</el-button>
              </el-col>
            </template>
          </el-form-item>
        </el-col>

        <el-col :span="24">
          <template>
            <el-alert
              title="附件要求"
              type="success"
              :description="fileAskContent"
              show-icon>
            </el-alert>
          </template>
        </el-col>
        <!--独立附件start-->
        <el-col :span="24">
          <el-form-item label="项目进展报告" prop="reportSalesName">
            <el-upload
              class="upload-demo"
              :action="url"
              :data="{finishName: dataForm.finishName}"
              :on-success="successHandle1">
              <el-button size="small" type="primary">点击上传</el-button>
              <label>
                （以附件形式上传项目进展报告的扫描件）
              </label>
            </el-upload>
          </el-form-item>
        </el-col>
        <!--独立附件end-->
        <!--<el-col :span="24">-->
          <!--<el-form-item label="附件列表" prop="attachLists">-->
            <!--<template v-for="(item,index) in attachLists" v-if="item.isDel !== 1">-->
              <!--<el-col :span="24">-->
                <!--<el-tag style="margin-right: 1rem"-->
                        <!--v-text="item.attachName">-->
                <!--</el-tag>-->
                <!--<el-button size="mini" type="danger" @click="delAttach(item, index)">删除</el-button>-->
              <!--</el-col>-->
            <!--</template>-->
          <!--</el-form-item>-->
        <!--</el-col>-->
        <!--<el-col :span="24">-->
          <!--<el-form-item label="文件上传">-->
            <!--<el-upload-->
              <!--multiple-->
              <!--ref="upLoadFiles"-->
              <!--list-type="card"-->
              <!--:data="upLoadData"-->
              <!--:action="upLoadUrl"-->
              <!--:on-preview="upLoadPreview"-->
              <!--:on-remove="upLoadRemove"-->
              <!--:on-success="upLoadSuccess"-->
              <!--:on-change="upLoadChange"-->
              <!--:file-list="fileList"-->
              <!--:auto-upload="false">-->
              <!--<el-button slot="trigger" size="small" type="primary">选取文件</el-button>-->
              <!--<el-button style="margin-left: 10px;" size="small" type="success" @click="upLoadSubmit">添加到附件列表</el-button>-->
              <!--<div slot="tip" class="el-upload__tip">注意文件需要添加到附件列表后确定保存才能添加成功</div>-->
            <!--</el-upload>-->
            <!--&lt;!&ndash;<input type="file" name="file" hidden/>&ndash;&gt;-->
          <!--</el-form-item>-->
        <!--</el-col>-->
      </el-form>
    </el-row>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
    <teacher-add-or-update v-if="teacherAddOrUpdateVisible" ref="teacherAddOrUpdate" @refreshDataList="teacherRef"></teacher-add-or-update>
    <person-add-or-update v-if="personAddOrUpdateVisible" ref="personAddOrUpdate" @refreshDataList="personRef"></person-add-or-update>
    <!--<award-add-or-update v-if="awardAddOrUpdateVisible" ref="awardAddOrUpdate" @refreshDataList="awardRef"></award-add-or-update>-->
    <staff-add-or-update v-if="staffAddOrUpdateVisible" ref="staffAddOrUpdate" @refreshDataList="staffRef"></staff-add-or-update>
  </el-dialog>
</template>

<script>
  import TeacherAddOrUpdate from './teacher-add-or-update'
  import StaffAddOrUpdate from './staff-add-or-update'
  export default {
    components: {
      StaffAddOrUpdate,
      TeacherAddOrUpdate
    },
    data () {
      var validateTeacher = (rule, value, callback) => {
        if (this.teacherLists.length === 0) {
          callback(new Error('指导老师信息不能为空'))
        } else {
          let notNull = false
          for (let index = 0; index < this.teacherLists.length; index++) {
            if (this.teacherLists[index].isDel === 0) {
              notNull = true
            }
          }
          if (notNull) {
            callback()
          } else {
            callback(new Error('指导老师信息不能为空'))
          }
        }
      }
      return {
        url: '',
        upLoadUrl: '',
        upLoadData: {},
        fileAskContent: '无',
        tables: [],
        fileList: [],
        teacherLists: [],
        personInfoList: [],
        attachLists: [],
        // awardLists: [],
        staffInfoLists: [],
        // eventLists: [],
        dataListLoading: false,
        teacherAddOrUpdateVisible: false,
        isTeacherInfoNullVisible: false,
        personAddOrUpdateVisible: false,
        legalAddOrUpdateVisible: false,
        staffAddOrUpdateVisible: false,
        moneyAddOrUpdateVisible: false,
        stationVisible: false,
        visible: false,
        staffInfoVisible: false,
        leadVisible: false,
        ptStaffInfoVisible: false,
        projectTeaInfoVisible: false,
        stationList: [],
        userTeacherInfoEntities: [],
        statusList: [{value: 1, label: '是'}, {value: 2, label: '否'}],
        finishTypeList: [
          {value: 1, label: '创新训练项目'},
          {value: 2, label: '创业训练项目'},
          {value: 3, label: '创业实践项目'}
        ],
        dataForm: {
          finishId: '',
          projectUserId: this.$store.state.user.id,
          finishName: '',
          finishType: '',
          finishExpect: '',
          projectFinishApplyStatus: 0,
          isUpdate: 0,
          isDel: 0
        },
        dataRule: {
          finishName: [
            { required: true, message: '申报名称不能为空', trigger: 'blur' }
          ],
          // eventId: [
          //   { required: true, message: '请选择结题赛事', trigger: 'blur' }
          // ],
          finishType: [
            { required: true, message: '请选择结题类型', trigger: 'blur' }
          ],
          teacherLists: [
            { validator: validateTeacher, trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.url = this.$http.adornUrl(`/innovate/finish/attach/upload?token=${this.$cookie.get('token')}`)
        this.visible = true
        this.dataListLoading = true
        this.dataForm.finishId = id || 0
        if (this.isAuth('innovate:station:list')) {
          this.$http({
            url: this.$http.adornUrl(`/innovate/base/station/list`),
            method: 'get',
            params: this.$http.adornParams()
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.stationList = data.page.list
            }
          })
        }
        this.$nextTick(() => {
          this.$http({
            url: this.$http.adornUrl(`/innovate/use/teacher/teacher`),
            method: 'get',
            params: this.$http.adornParams({
            })
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.userTeacherInfoEntities = data.sysTeacherEntities
            }
          })
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.finishId) {
            this.$http({
              url: this.$http.adornUrl(`/innovate/finish/info/info`),
              method: 'get',
              params: this.$http.adornParams({
                'finishId': this.dataForm.finishId,
                'apply': 'project_finish_apply_status'
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm = data.finishInfo.finishInfoEntity
                this.teacherLists = data.finishInfo.finishTeacherEntities
                this.personInfoList = []
                this.attachLists = data.finishInfo.finishAttachEntities
                this.staffInfoLists = data.finishInfo.finishStaffInfoEntities
                this.dataForm.finishNoPass = 0
                this.isTeacherInfoNull()
                this.dataListLoading = false
              }
            })
          } else {
            this.dataListLoading = false
          }
        })
        // 获取文件要求：类型=>1 大创,2 中期检查,3 赛事,4 结题
        this.$http({
          url: this.$http.adornUrl(`/innovate/sys/file/ask/query`),
          method: 'get',
          params: this.$http.adornParams({
            'fileAskType': 4,
            'fileAskTime': new Date().getFullYear()
          })
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.fileAskContent = data.fileAsk.fileAskContent
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          this.dataForm.finishId = this.dataForm.finishId || undefined
          if (valid) {
            if (this.dataForm.finishId) {
              this.dataForm.projectSetDate = Number(this.dataForm.projectSetDate)
              this.dataForm.projectInDate = Number(this.dataForm.projectInDate)
              this.dataForm.projectRegDate = Number(this.dataForm.projectRegDate)
            }
            this.dataForm.isUpdate = 0
            this.$http({
              url: this.$http.adornUrl(`/innovate/finish/info/${!this.dataForm.finishId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'finishInfoEntity': this.dataForm,
                'projectLegalInfoEntities': this.legalLists,
                'userPersonInfoEntities': this.personInfoList,
                'finishTeacherEntities': this.teacherLists,
                'finishAttachEntities': this.attachLists,
                'finishStaffInfoEntities': this.staffInfoLists
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      },
      // 子组件的各类方法
      addVisible (list) {
        let isDelNum = 0
        let visible = false
        for (let index in list) {
          if (list.length > 0 && list[index].isDel === 1) {
            isDelNum++
          }
        }
        if (isDelNum === list.length) {
          visible = true
        }
        return visible
      },
      addTeacher (item, index) {
        this.teacherAddOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs['teacherAddOrUpdate'].init(item, index + 1)
        })
        this.isTeacherInfoNull()
      },
      delTeacher: function (data, index) {
        data.isDel = 1
        this.teacherLists[index] = data
        this.isTeacherInfoNull()
      },
      teacherRef (data, index) {
        data.finishId = this.dataForm.finishId
        this.teacherAddOrUpdateVisible = false
        if (index) {
          this.teacherLists[index - 1] = data
        } else {
          this.teacherLists.push(data)
        }
        this.isTeacherInfoNull()
      },
      isTeacherInfoNull () {
        this.isTeacherInfoNullVisible = false
        var teacherNum = 0
        // 被选中的老师数量
        var teacherLength = 0
        for (let item in this.teacherLists) {
          for (let teacher in this.userTeacherInfoEntities) {
            if (this.teacherLists[item].userId !== null) {
              if (this.userTeacherInfoEntities[teacher].userId === this.teacherLists[item].userId && this.teacherLists[item].isDel !== 1) {
                teacherNum++
              }
            }
          }
          if (this.teacherLists[item].isDel !== 1) {
            teacherLength++
          }
        }
        if (teacherNum < teacherLength) {
          this.isTeacherInfoNullVisible = true
        }
      },
      personRef (data, index) {
        data.finishId = this.dataForm.finishId
        this.personAddOrUpdateVisible = false
        if (index) {
          this.personInfoList[index - 1] = data
        } else {
          this.personInfoList.push(data)
        }
      },
      addAward (item, index) {
        this.awardAddOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs['awardAddOrUpdate'].init(item, index + 1)
        })
      },
      delAward: function (data, index) {
        data.isDel = 1
        this.awardLists[index] = data
      },
      awardRef (data, index) {
        data.finishId = this.dataForm.finishId
        this.awardAddOrUpdateVisible = false
        if (index) {
          this.awardLists[index - 1] = data
        } else {
          this.awardLists.push(data)
        }
      },
      addStaff (item, index) {
        this.staffAddOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs['staffAddOrUpdate'].init(item, index + 1)
        })
      },
      delStaff: function (data, index) {
        data.isDel = 1
        this.staffInfoLists[index] = data
      },
      staffRef (data, index) {
        data.finishId = this.dataForm.finishId
        this.staffAddOrUpdateVisible = false
        if (index) {
          this.staffInfoLists[index - 1] = data
        } else {
          this.staffInfoLists.push(data)
        }
      },
      // delAttach: function (data, index) {
      //   if (data.isDel !== 1) {
      //     data.isDel = 1
      //     this.attachLists[index] = data
      //   } else {
      //     this.attachLists.splice(index, 1)
      //   }
      // },

      // upLoadSubmit () {
      //   this.$refs.upLoadFiles.submit()
      // },
      // upLoadChange () {
      //   this.upLoadData = {
      //     'finishName': this.dataForm.finishName,
      //     'token': this.$cookie.get('token')
      //   }
      //   this.upLoadUrl = this.$http.adornUrl(`/innovate/finish/attach/upload`)
      // },
      // upLoadRemove (file, fileList) {
      //   console.log(file, fileList)
      // },
      // upLoadPreview (file) {
      //   console.log(file)
      // },
      // upLoadSuccess (data) {
      //   if (data && data.code === 0) {
      //     this.attachLists.push(data.finishAttachEntity)
      //   } else {
      //     this.$message.error(data.msg)
      //   }
      // },

      closeDialog () {
        this.visible = false
        this.$emit('refreshDataList')
      },
      // 上传成功
      successHandle1 (response, file, fileList) {
        if (response && response.code === 0) {
          this.attachLists.push(response.finishAttachEntity)
        } else {
          this.$message.error(response.msg)
        }
      }

    }
  }
</script>

