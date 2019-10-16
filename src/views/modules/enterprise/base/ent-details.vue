<template>
  <el-dialog
    :title="'企业审核'"
    :close-on-click-modal="false"
    width="60%"
    :visible.sync="visible">
    <el-form :model="dataForm" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="150px">
      <el-form-item label="企业名称" prop="entName">
        <el-input v-model="dataForm.entName" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="法人代表" prop="entCorporate">
        <el-input v-model="dataForm.entCorporate" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="经营范围" prop="entBusiness">
        <el-input type="textarea" v-model="dataForm.entBusiness" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="统一信用代码" prop="entCode">
        <el-input v-model="dataForm.entCode" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="注册资本" prop="entRegister">
        <el-input v-model="dataForm.entRegister" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="成立时间" prop="entFoundingTime">
        <el-date-picker
          v-model="dataForm.entFoundingTime"
          :readonly="true"
          type="datetime"
          format="yyyy-MM-dd HH:mm:ss"
          value-format="timestamp"
          placeholder="选择日期">
        </el-date-picker>
      </el-form-item>
      <el-form-item label="企业类型" prop="entType">
        <el-input v-model="dataForm.entType" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="经营状态" prop="entStatus">
        <el-input v-model="dataForm.entStatus" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="注册地址" prop="entRegisterAddress">
        <el-input type="textarea" v-model="dataForm.entRegisterAddress" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="企业地址" prop="entAddress">
        <el-input type="textarea" v-model="dataForm.entAddress" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="企业介绍" prop="entIntroduce">
        <el-input type="textarea" v-model="dataForm.entIntroduce" :readonly="true"></el-input>
      </el-form-item>
      <el-form-item label="是否高新区" prop="newHighZones">
        <el-radio-group v-model="dataForm.newHighZones" size="small" :readonly="true">
          <el-radio border label="0" :readonly="true">是</el-radio>
          <el-radio border label="1" :readonly="true">否</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="入驻申请时间" prop="entInTime">
        <el-date-picker
          v-model="dataForm.entInTime"
          :readonly="true"
          type="datetime"
          format="yyyy-MM-dd HH:mm:ss"
          value-format="timestamp"
          placeholder="选择日期">
        </el-date-picker>
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
          entInfoId: 0,
          entName: '',
          entCorporate: '',
          entBusiness: '',
          entCode: '',
          entRegister: '',
          entFoundingTime: '',
          entType: '',
          entStatus: '',
          entRegisterAddress: '',
          entAddress: '',
          entIntroduce: '',
          newHighZones: '',
          entInTime: '',
          inApply: '1'
        }
      }
    },
    methods: {
      init (id, hasApply) {
        this.visible = true
        this.dataForm.entInfoId = id || 0
        this.dataForm.inApply = hasApply || 1
        if (this.dataForm.entInfoId) {
          this.$http({
            url: this.$http.adornUrl(`/enterprise/info/info/${this.dataForm.entInfoId}/${this.dataForm.inApply}`),
            method: 'get',
            params: this.$http.adornParams()
          }).then(({data}) => {
            console.log(this.dataForm.entInfoId)
            console.log(this.dataForm.inApply)
            if (data && data.code === 0) {
              this.dataForm.entName = data.data.entName
              this.dataForm.entCorporate = data.data.entCorporate
              this.dataForm.entBusiness = data.data.entBusiness
              this.dataForm.entCode = data.data.entCode
              this.dataForm.entRegister = data.data.entRegister
              this.dataForm.entFoundingTime = new Date(data.data.entFoundingTime)
              this.dataForm.entType = data.data.entType
              this.dataForm.entStatus = data.data.entStatus
              this.dataForm.entRegisterAddress = data.data.entRegisterAddress
              this.dataForm.entAddress = data.data.entAddress
              this.dataForm.entIntroduce = data.data.entIntroduce
              this.dataForm.newHighZones = data.data.newHighZones
              this.dataForm.entInTime = new Date(data.data.entInTime)
              this.dataForm.inApply = data.data.inApply
            }
          })
        }
      }
    }
  }
</script>
