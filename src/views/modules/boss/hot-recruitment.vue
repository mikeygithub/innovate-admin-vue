<template>
  <!-- 城市热招 -->
  <div class="app-wrap" v-if="!hide">
    <div class="app-w app-qy mar-t30">
      <h3 class="text-not-wrap">
        <span class="cur">{{datas.title}}</span>
      </h3>
      <!-- 二级关联 -->
      <div class="app-hot-box clearfix box-s-b">
        <!-- 列表项 -->
        <ul class="show clearfix">
          <!-- 子项 -->
          <li v-if="datas" v-for="(item, index) in datas.imgList" :key="index">
            <a :href="item.href" target="_blink">
              <div class="hot-bg"></div>
              <img :src="item.src" alt="">
            </a>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  // 城市热招
  export default {
    props: {hide: Boolean},
    data () {
      return {
        datas: {
          title: '梧州学院 | 双创成果',
          imgList: [
            {src: '/src/assets/img/city/city1.jpg', href: 'http://cxcy.gxuwz.edu.cn/info/1005/1551.htm'},
            {src: '/src/assets/img/city/city2.jpg', href: 'http://cxcy.gxuwz.edu.cn/info/1090/1193.htm'},
            {src: '/src/assets/img/city/city3.jpg', href: 'http://cxcy.gxuwz.edu.cn/info/1091/1663.htm'},
            {src: '/src/assets/img/city/city4.jpeg', href: 'http://cxcy.gxuwz.edu.cn/info/1091/1623.htm'},
            {src: '/src/assets/img/city/city5.jpg', href: 'http://cxcy.gxuwz.edu.cn/info/1091/1623.htm'},
            {src: '/src/assets/img/city/city6.jpg', href: 'http://cxcy.gxuwz.edu.cn/info/1091/1622.htm'},
            {src: '/src/assets/img/city/city7.jpg', href: 'http://cxcy.gxuwz.edu.cn/info/1091/1622.htm'},
            {src: '/src/assets/img/city/city8.jpg', href: 'http://cxcy.gxuwz.edu.cn/info/1091/1621.htm'},
            {src: '/src/assets/img/city/city9.png', href: 'http://cxcy.gxuwz.edu.cn/info/1005/1502.htm'},
            {src: '/src/assets/img/city/city10.jpg', href: 'http://cxcy.gxuwz.edu.cn/info/1005/1240.htm'},
            {src: '/src/assets/img/city/city11.jpg', href: 'http://cxcy.gxuwz.edu.cn/info/1005/1203.htm'},
            {src: '/src/assets/img/city/city12.jpg', href: 'http://cxcy.gxuwz.edu.cn/info/1005/1190.htm'}
          ]
        }
      }
    }
  }
</script>

<style scoped>

</style>
