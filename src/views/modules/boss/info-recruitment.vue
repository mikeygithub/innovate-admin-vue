<template>
  <div class="app-wrap" v-if="!hide">
    <div class="app-w app-zp mar-t30 user-select">
      <h3 class="text-not-wrap" v-if="menu">
        <span v-for="(item, index) in menu" :key="index" :class="{cur: item.cur}" @click="getListInfo(item.name, index)">{{item.name}}</span>
      </h3>
      <!-- 二级关联 -->
      <div class="app-zp-box clearfix box-s-b">
        <!-- 列表项 -->
        <ul class="show clearfix">
          <!-- 子项 -->
          <li v-for="(item, index) in list" :key="index">
            <div class="sub-li">
              <a :href="item.url" class="job-info" :target="item.target || '_blank'">
                <p>{{item.title && item.title.name}}<span class="salary">{{item.title && item.title.value}}</span></p>
                <p class="job-text">{{item.job && item.job.name}}<span class="vline"></span>{{item.job && item.job.years}}<span class="vline"></span>{{item.job && item.job.value}}</p>
              </a>
              <a :href="item.url" class="user-info" :target="item.target || '_blank'">
                <p>
                  <img :src="item.company && item.company.img" :alt="item.company && item.company.img" :title="item.company && item.company.img">{{item.company && item.company.name}}
                  <span class="user-text">{{item.company && item.company.link}}<span class="vline"></span>{{item.company && item.company.position}}</span>
                </p>
              </a>
            </div>
          </li>
        </ul>
      </div>
      <p class="common-tab-more"  v-if="more"><a class="btn btn-outline" :href="more.url" :target="more.target || '_blank'">{{more.name}}</a></p>
    </div>
  </div>
</template>

<script>
  // 招聘信息
  export default {
    props: {hide: Boolean},
    data () {
      return {
        list: [{url: '#', target: '_blank', title: {name: '课程顾问/招生顾问/课程销售+教育培训行业', value: '10-15K'}, job: {name: '上海', value: '学历不限', years: '1-3年'}, company: {img: '/src/assets/img/users.jpg', name: '看看童话', link: '吴莹', position: '人事经理'}},
          {url: '#', target: '_blank', title: {name: '课程顾问/招生顾问/课程销售+教育培训行业', value: '10-15K'}, job: {name: '上海', value: '学历不限', years: '1-3年'}, company: {img: '/src/assets/img/users.jpg', name: '看看童话', link: '吴莹', position: '人事经理'}},
          {url: '#', target: '_blank', title: {name: '课程顾问/招生顾问/课程销售+教育培训行业', value: '10-15K'}, job: {name: '上海', value: '学历不限', years: '1-3年'}, company: {img: '/src/assets/img/users.jpg', name: '看看童话', link: '吴莹', position: '人事经理'}},
          {url: '#', target: '_blank', title: {name: '课程顾问/招生顾问/课程销售+教育培训行业', value: '10-15K'}, job: {name: '上海', value: '学历不限', years: '1-3年'}, company: {img: '/src/assets/img/users.jpg', name: '看看童话', link: '吴莹', position: '人事经理'}},
          {url: '#', target: '_blank', title: {name: '课程顾问/招生顾问/课程销售+教育培训行业', value: '10-15K'}, job: {name: '上海', value: '学历不限', years: '1-3年'}, company: {img: '/src/assets/img/users.jpg', name: '看看童话', link: '吴莹', position: '人事经理'}},
          {url: '#', target: '_blank', title: {name: '课程顾问/招生顾问/课程销售+教育培训行业', value: '10-15K'}, job: {name: '上海', value: '学历不限', years: '1-3年'}, company: {img: '/src/assets/img/users.jpg', name: '看看童话', link: '吴莹', position: '人事经理'}},
          {url: '#', target: '_blank', title: {name: '课程顾问/招生顾问/课程销售+教育培训行业', value: '10-15K'}, job: {name: '上海', value: '学历不限', years: '1-3年'}, company: {img: '/src/assets/img/users.jpg', name: '看看童话', link: '吴莹', position: '人事经理'}},
          {url: '#', target: '_blank', title: {name: '课程顾问/招生顾问/课程销售+教育培训行业', value: '10-15K'}, job: {name: '上海', value: '学历不限', years: '1-3年'}, company: {img: '/src/assets/img/users.jpg', name: '看看童话', link: '吴莹', position: '人事经理'}},
          {url: '#', target: '_blank', title: {name: '课程顾问/招生顾问/课程销售+教育培训行业', value: '10-15K'}, job: {name: '上海', value: '学历不限', years: '1-3年'}, company: {img: '/src/assets/img/users.jpg', name: '看看童话', link: '吴莹', position: '人事经理'}},
          {url: '#', target: '_blank', title: {name: '课程顾问/招生顾问/课程销售+教育培训行业', value: '10-15K'}, job: {name: '上海', value: '学历不限', years: '1-3年'}, company: {img: '/src/assets/img/users.jpg', name: '看看童话', link: '吴莹', position: '人事经理'}},
          {url: '#', target: '_blank', title: {name: '课程顾问/招生顾问/课程销售+教育培训行业', value: '10-15K'}, job: {name: '上海', value: '学历不限', years: '1-3年'}, company: {img: '/src/assets/img/users.jpg', name: '看看童话', link: '吴莹', position: '人事经理'}},
          {url: '#', target: '_blank', title: {name: '课程顾问/招生顾问/课程销售+教育培训行业', value: '10-15K'}, job: {name: '上海', value: '学历不限', years: '1-3年'}, company: {img: '/src/assets/img/users.jpg', name: '看看童话', link: '吴莹', position: '人事经理'}}],
        menu: [{name: 'IT·互联网', cur: true}, {name: '金融', cur: false}, {name: '房地产·建筑', cur: false}, {name: '教育培训', cur: false}, {name: '汽车', cur: false},
          {name: '娱乐传媒', cur: false}, {name: '医疗健康', cur: false}, {name: '法律咨询', cur: false}, {name: '供应链·物流', cur: false}, {name: '采购贸易', cur: false}],
        more: {name: '查看更多', url: '#'}
      }
    },
    methods: {
      getListInfo (type, index) {
        // console.log(type, index)
        let that = this
        this.menu.forEach((item, aindex, arr) => {
          // console.log(item, aindex, arr)
          item['cur'] = false
          if (index === aindex) {
            that.menu[aindex].cur = true
            // TODO请求网络数据，回填list
          }
        })
      }
    }
  }
</script>

<style scoped>

</style>
