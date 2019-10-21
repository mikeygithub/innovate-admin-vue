<template>
  <div>
  <el-dialog
    :close-on-click-modal=" visible = false"
    width="60%"
    :visible.sync="visible">
    <!-- 详情 -->
    <div class="info-box">
      <!-- 顶部 -->
      <div class="info-top">
        <!-- 左边 -->
        <div class="top-left s">
          <h4>java（银行项目）java（银行项目）java（银行项目）java（银行项目）</h4>
          <p>软通动力。招聘顾问。操红雷软通动力。招聘顾问。操红雷软通动力。招聘顾问。操红雷软通动力。招聘顾问。操红雷</p>
        </div>
        <!-- 右边 -->
        <div class="top-right s">
          <div class="t-r-b">
            <span class="info-btn">立即加入</span>
          </div>
        </div>
        <div style="clear: both;"></div>
        <!-- 主要主题项目内容 -->
        <div class="info-content">
          <div class="detail-bottom-title">职位描述</div>
          <div class="detail-bottom-text">1.大专及以上学历，计算机、管理类专业优先<br>2.熟悉掌握PPT、Word、Excel表格等办公软件<br>3.抗压能力强，有良好的沟通协调能力和团队协<br>助精神<br>4.有较强的推动能力<br>5.能够适应较高的工作强度<br>1）项目计划及进度任务、风险问题跟进<br>2）项目过程数据收集整理<br>3）项目规范流程建设<br>4）项目团队建设<br>5）项目过程风险识别及优化建议提出<br>6）项目规范化审计<br>7）项目会议组织<br>8）项目过程交付文档存档
          </div>
        </div>
      </div>
      <!--  -->
    </div>
  </el-dialog>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        visible: false
      }
    },
    methods: {
      init (hasType, id) {
        this.visible = true
        this.proInfoId = id
      }
    }
  }
</script>

<style >
  .s{box-sizing: border-box;}
  .info-box{width: 100%;height: 400px;margin: 100px auto;background-color: pink;}
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
