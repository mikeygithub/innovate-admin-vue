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
      init (id) {
        this.visible = true
        this.dataForm.proInfoId = id || 0
        if (this.dataForm.proInfoId) {
          this.$http({
            url: this.$http.adornUrl(`/enterprise/project/info/info/${this.dataForm.proInfoId}`),
            method: 'get',
            params: this.$http.adornParams()
          }).then(({data}) => {
            console.log(data)
            if (data && data.code === 0) {
              this.dataForm = data.entProjectInfo
            }
          })
        }
      }
    }
  }
</script>
