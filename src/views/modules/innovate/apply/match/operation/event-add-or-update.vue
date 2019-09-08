<!--管理员账号创建比赛报名人口，命名赛事-->
<template>
  <el-dialog
    append-to-body
    :title="!id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="150px">
      <el-form-item label="赛事名称" prop="eventName">
        <el-input v-model="dataForm.eventName" placeholder="请输入"></el-input>
      </el-form-item>
      <!--<el-form-item label="比赛参加人数" prop="eventPeople">-->
        <!--<el-input v-model.number="dataForm.eventPeople" placeholder="请输入"></el-input>-->
      <!--</el-form-item>-->
      <!--<el-form-item label="获奖数量" prop="eventAwardNum">-->
        <!--<el-input v-model.number="dataForm.eventAwardNum" placeholder="请输入"></el-input>-->
      <!--</el-form-item>-->
      <!--<el-form-item label="项目团队数量" prop="eventProjectNum">-->
        <!--<el-input v-model.number="dataForm.eventProjectNum" placeholder="请输入"></el-input>-->
      <!--</el-form-item>-->
      <!--<el-form-item label="奖金数量（万元）" prop="eventAwardMoney">-->
        <!--<el-input v-model.number="dataForm.eventAwardMoney" placeholder="请输入"></el-input>-->
      <!--</el-form-item>-->
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()" :loading="addLoading">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  // import { isFloatNumber } from '@/utils/validate'
  export default {
    name: 'event-add-or-update',
    data () {
      // var validateFloatNumber = (rule, value, callback) => {
      //   if (!isFloatNumber(value)) {
      //     callback(new Error('至少保留小数点后一位'))
      //   } else {
      //     callback()
      //   }
      // }
      return {
        addLoading: false,
        visible: false,
        // awardFileName: {
        //   matchName: ''
        // },
        // typeList: [{
        //   value: 1,
        //   label: '国际级'
        // }, {
        //   value: 2,
        //   label: '国家级'
        // }, {
        //   value: 3,
        //   label: '省级'
        // }, {
        //   value: 4,
        //   label: '市厅级'
        // }, {
        //   value: 5,
        //   label: '县局级'
        // }, {
        //   value: 6,
        //   label: '校级'
        // }],
        // rankList: [{
        //   value: 1,
        //   label: '特等奖'
        // }, {
        //   value: 2,
        //   label: '一等奖'
        // }, {
        //   value: 3,
        //   label: '二等奖'
        // }, {
        //   value: 4,
        //   label: '三等奖'
        // }, {
        //   value: 5,
        //   label: '优秀奖'
        // }, {
        //   value: 6,
        //   label: '金奖'
        // }, {
        //   value: 7,
        //   label: '银奖'
        // }, {
        //   value: 8,
        //   label: '铜奖'
        // }, {
        //   value: 9,
        //   label: '第一名'
        // }, {
        //   value: 10,
        //   label: '第二名'
        // }, {
        //   value: 11,
        //   label: '第三名'
        // }, {
        //   value: 12,
        //   label: '第四名'
        // }, {
        //   value: 13,
        //   label: '第五名'
        // }, {
        //   value: 14,
        //   label: '其他'
        // }],
        id: 0,
        fileList: [],
        dataForm: {
          eventId: '',
          eventName: ''
          // eventPeople: '',
          // eventAwardNum: '',
          // eventProjectNum: '',
          // eventAwardMoney: ''
        },
        dataRule: {
          eventName: [
            { required: true, message: '赛事名称不能为空', trigger: 'blur' }
          ]
          // eventPeople: [
          //   { required: true, message: '比赛参加人数不能为空', trigger: 'blur' },
          //   {type: 'number', message: '必须为数字', trigger: 'blur'}
          // ],
          // eventAwardNum: [
          //   { required: true, message: '获奖数量不能为空', trigger: 'blur' },
          //   {type: 'number', message: '必须为数字', trigger: 'blur'}
          // ],
          // eventProjectNum: [
          //   { required: true, message: '项目团队数量不能为空', trigger: 'blur' },
          //   {type: 'number', message: '必须为数字', trigger: 'blur'}
          // ],
          // eventAwardMoney: [
          //   { required: true, message: '奖金数量不能为空', trigger: 'blur' },
          //   {type: 'number', message: '必须为数字', trigger: 'blur'},
          //   { validator: validateFloatNumber, trigger: 'blur' }
          // ]
        }
      }
    },
    methods: {
      init (index) {
        this.visible = true
        this.id = index || 0
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.id) {
            this.$http({
              url: this.$http.adornUrl(`/innovate/match/event/info/` + index),
              method: 'get',
              params: this.$http.adornParams({
                'eventId': this.id,
                'apply': 'project_match_apply_status'
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm = data.matchEventEntity
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          this.dataForm.eventId = this.id || undefined
          if (valid) {
            this.addLoading = true
            this.$http({
              url: this.$http.adornUrl(`/innovate/match/event/${!this.dataForm.eventId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData(this.dataForm)
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
      }
    }
  }
</script>
