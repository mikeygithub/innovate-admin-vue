<template>
  <div class="app-wrap app-outbox" v-if="!hide">
    <div class="app-msgbox app-w clearfix">
      <!-- 左边 -->
      <div class="app-mleft clearfix fl-l">
        <img :src="bg" alt="" title="">
      </div>
      <!-- 右边 -->
      <div class="app-mright clearfix fl-r position-r">
        <img :src="resume.src" alt="" title="">
        <a href="javascript:;" @click="uploadResume" class="btn btn-upload-jl btn-to-upload" >上传附件简历</a>
      </div>
    </div>
  </div>
</template>

<script>
  // 顶部消息通知
  export default {
    name: 'resume-tip',
    props: {hide: Boolean},
    data () {
      return {
        bg: '/src/assets/img/fast-reg.png',
        resume: {src: '/src/assets/img/fast-reg-text.png'}
      }
    },
    methods: {
      uploadResume () {
        this.$message.success('你点击了上传简历')
      }
    }
  }
</script>

<style scoped>

</style>
