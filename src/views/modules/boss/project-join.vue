<template>
  <el-dialog
    :title="'项目信息'"
    :width="'60%'"
    v-loading="loading"
    :visible.sync="visible">
    <!-- 详情 -->
    <div class="info-box" v-if="result">
      <!-- 顶部 -->
      <div class="info-top">
        <!-- 左边 -->
        <div class="top-left s">
          <h4>{{result.proName}}</h4>
          <p>项目类型:{{result.proType}} - 项目来源:{{result.proOrigin}} - 项目经费:{{result.proutlay}}</p>
        </div>
        <!-- 右边 -->
        <div class="top-right s">
          <div class="t-r-b">
            <span class="info-btn" @click="join2">立即加入</span>
          </div>
        </div>
        <div style="clear: both;"></div>
        <!-- 主要主题项目内容 -->
        <div class="info-content">
          <div class="detail-bottom-title">项目介绍</div>
          <div class="detail-bottom-text">{{result.proIntroduce}}</div>
        </div>
      </div>
    </div>

  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        loading: true,
        visible: false,
        proInfoId: 0,
        result: null
      }
    },
    methods: {
      // 加入项目
      join2 () {
        console.log('加入项目', this.result.proInfoId)
        this.$http({
          url: this.$http.adornUrl(`/enterprise/person/cooperation/save`),
          method: 'get',
          params: this.$http.adornParams({
            'proInfoId': this.result.proInfoId
          })
        }).then(({data}) => {
          console.log(data)
          this.loading = false
          if (data && data.code === 0) {
            this.$message.success(data.msg)
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      init (hasType, id) {
        console.log(hasType, id)
        this.visible = true
        this.proInfoId = id
        this.$http({
          url: this.$http.adornUrl(`/webpage/projectInfo/${this.proInfoId}/`),
          method: 'get',
          params: this.$http.adornParams()
        }).then(({data}) => {
          console.log(data)
          this.loading = false
          if (data && data.code === 0) {
            this.result = data.data
          }
        })
      }
    }
  }
</script>

<style >
  .s{box-sizing: border-box;}
  .info-box{width: 100%;}
  .info-top{padding: 20px;background-color: #f5f8fc;}
  .top-left{float: left;width: 50%;color: #666;font-size: 22px;}
  .top-left h4{font-weight: 400;line-height: 50px;width: 100%;overflow: hidden;text-overflow: ellipsis;white-space: nowrap;}
  .top-left p{font-weight: 400;font-size: 18px;color: #aaa;line-height: 20px;width: 100%;overflow: hidden;text-overflow: ellipsis;white-space: nowrap;}
  .top-right{margin-left: 50%;}
  .t-r-b{margin-top: 20px;text-align: right;}
  .info-btn{ height: 30px;line-height: 30px;display: inline-block;font-size: 13px;min-width: 85px;    color: #fff;letter-spacing: 1px;background: #5dd5c8;text-align: center;cursor: pointer;border: 1px #5dd5c8 solid;margin-top: 0;}
  .info-btn:hover{background-color: #84ece1;color: #eee;}

  .info-content{margin-top:20px;background: #fff;padding: 0 20px 23px;font-size: 13px;}
  .detail-bottom-title{padding: 10px 0 0;font-size: 14px;color: #414a60;}
  .detail-bottom-text{max-height: 260px;overflow: hidden;display: -webkit-box;-webkit-box-orient: vertical;-webkit-line-clamp: 10;font-size: 14px;    white-space: normal; color: #abafba;line-height: 26px;}

</style>
