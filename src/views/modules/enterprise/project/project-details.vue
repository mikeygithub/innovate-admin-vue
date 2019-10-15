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
    <el-collapse v-if="dataForm.entInfoId" v-model="activeNames" content="true" @change="handleChange">
      <el-collapse-item title="企业详情" name="1">
        <div>与现实生活一致：与现实生活的流程、逻辑保持一致，遵循用户习惯的语言和概念；</div>
        <div>在界面中一致：所有的元素和结构需保持一致，比如：设计样式、图标和文本、元素的位置等。</div>
      </el-collapse-item>
    </el-collapse>
    <el-collapse v-if="dataForm.userPerId" v-model="activeNames" content="true" @change="handleChange">
      <el-collapse-item title="发布者详情" name="2">
        <div>与现实生活一致：与现实生活的流程、逻辑保持一致，遵循用户习惯的语言和概念；</div>
        <div>在界面中一致：所有的元素和结构需保持一致，比如：设计样式、图标和文本、元素的位置等。</div>
      </el-collapse-item>
    </el-collapse>
    <el-collapse v-if="dataForm.userTeacherId" v-model="activeNames" content="true" @change="handleChange">
      <el-collapse-item title="发布者详情" name="3">
        <div>与现实生活一致：与现实生活的流程、逻辑保持一致，遵循用户习惯的语言和概念；</div>
        <div>在界面中一致：所有的元素和结构需保持一致，比如：设计样式、图标和文本、元素的位置等。</div>
      </el-collapse-item>
    </el-collapse>
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
    methods: {
      init (hasType, id) {
        this.visible = true
        this.dataForm.proInfoId = id
        this.dataForm.hasType = hasType
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
      handleChange (val) {
        console.log(val)
      }
    }
  }
</script>
