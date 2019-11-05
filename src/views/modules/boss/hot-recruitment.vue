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
          <li v-if="datas" v-for="(item, index) in datas.list" :key="index">
            <a href="#" target="_blink">
              <div class="hot-bg"></div>
              <img :src="item" alt="">
            </a>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  import CityImg from '@/assets/img/city_101010100.png'
  // 城市热招
  export default {
    props: {hide: Boolean},
    data () {
      return {
        datas: {
          title: '创新创业教育相关连接',
          list: [
            CityImg,
            CityImg,
            CityImg,
            CityImg,
            CityImg,
            CityImg,
            CityImg,
            CityImg,
            CityImg,
            CityImg,
            CityImg,
            CityImg]
            // '/src/assets/img/city_101010100.png',
            // '/src/assets/img/city_101010100.png',
            // '/src/assets/img/city_101010100.png',
            // '/src/assets/img/city_101010100.png',
            // '/src/assets/img/city_101010100.png',
            // '/src/assets/img/city_101010100.png',
            // '/src/assets/img/city_101010100.png',
            // '/src/assets/img/city_101010100.png',
            // '/src/assets/img/city_101010100.png',
            // '/src/assets/img/city_101010100.png',
            // '/src/assets/img/city_101010100.png']
        }
      }
    }
  }
</script>

<style scoped>

</style>
