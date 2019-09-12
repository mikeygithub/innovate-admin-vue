<template>
  <div class="app-wrap" v-if="!hide">
    <div class="app-w app-menu clearfix">
      <!-- 左边 -->
      <div class="app-menu-left fl-l">
        <!-- 列表项 -->
        <div class="app-menu-item" v-for="(item, index) in left" :key="index" >
          <p>
            <i class="app-tright"></i>
            <b v-if="item && item.title">{{item.title}}</b>
            <span v-if="item && item.items" v-for="(itm, idx) in item.items" :key="idx"><a :href="itm.url" :target="itm.target || '_blank'">{{itm.name}}</a></span>
          </p>
          <!-- 二级菜单 -->
          <div class="app-menubox">
            <b v-if="item && item.title">{{item.title}}</b>
            <div class="app-menub-item clearfix" v-if="item.second" v-for="(itm, idx) in item.second" :key="idx" >
              <div class="app-menub-l fl-l" v-if="itm.name">{{itm.name}}</div>
              <div class="app-menub-r">
                <a v-if="itm.datas" v-for="(it, id) in itm.datas" :key="id" :href="it.url" :target="it.target || '_blank'">{{it.name}}</a>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- 右边 -->
      <div class="app-menu-right clearfix">
        <ul class="fl-l app-menu-rleft">
          <li v-if="right && right.left" v-for="(item, index) in right.left" :key="index"><a :href="item.url" :target="item.target || '_blank'"><img :src="item.src" :alt="item.src" :title="item.src"></a></li>
        </ul>
        <ul class="fl-l app-menu-rright">
          <li v-if="right && right.right" v-for="(item, index) in right.right" :key="index"><a :href="item.url" :target="item.target || '_blank'"><img :src="item.src" :alt="item.src" :title="item.src"></a></li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  // 快捷菜单
  export default {
    name: 'boss-menu',
    props: { hide: Boolean },
    data () {
      return {
        left: [
          {title: '技术',
            items: [
              {url: '#', target: '_blank', name: 'Java'},
              {url: '#', target: '_blank', name: 'PHP'},
              {url: '#', target: '_blank', name: 'Web前端'}],
            second: [
              {name: '后端开发',
                datas: [
                  {url: '#', target: '_blank', name: '后端开发'},
                  {url: '#', target: '_blank', name: 'Java'},
                  {url: '#', target: '_blank', name: 'C++'},
                  {url: '#', target: '_blank', name: 'PHP'},
                  {url: '#', target: '_blank', name: 'C'},
                  {url: '#', target: '_blank', name: 'C#'},
                  {url: '#', target: '_blank', name: '.NET'},
                  {url: '#', target: '_blank', name: 'Hadoop'},
                  {url: '#', target: '_blank', name: 'Python'},
                  {url: '#', target: '_blank', name: 'Delphi'},
                  {url: '#', target: '_blank', name: 'VB'},
                  {url: '#', target: '_blank', name: 'Perl'},
                  {url: '#', target: '_blank', name: 'Ruby'},
                  {url: '#', target: '_blank', name: 'Node.js'},
                  {url: '#', target: '_blank', name: 'Golang'},
                  {url: '#', target: '_blank', name: 'Erlang'},
                  {url: '#', target: '_blank', name: '语音/视频/图形开发'},
                  {url: '#', target: '_blank', name: '数据采集'}
                ]
              },
              {name: '前端开发',
                datas: [
                  {url: '#', target: '_blank', name: '后端开发'},
                  {url: '#', target: '_blank', name: 'Java'},
                  {url: '#', target: '_blank', name: 'C++'},
                  {url: '#', target: '_blank', name: 'PHP'},
                  {url: '#', target: '_blank', name: 'C'},
                  {url: '#', target: '_blank', name: 'C#'},
                  {url: '#', target: '_blank', name: '.NET'},
                  {url: '#', target: '_blank', name: 'Hadoop'},
                  {url: '#', target: '_blank', name: 'Python'},
                  {url: '#', target: '_blank', name: 'Delphi'},
                  {url: '#', target: '_blank', name: 'VB'},
                  {url: '#', target: '_blank', name: 'Perl'},
                  {url: '#', target: '_blank', name: 'Ruby'},
                  {url: '#', target: '_blank', name: 'Node.js'},
                  {url: '#', target: '_blank', name: 'Golang'},
                  {url: '#', target: '_blank', name: 'Erlang'},
                  {url: '#', target: '_blank', name: '语音/视频/图形开发'},
                  {url: '#', target: '_blank', name: '数据采集'}
                ]
              }
            ]
          },
          {title: '技术',
            items: [
              {url: '#', target: '_blank', name: 'Java'},
              {url: '#', target: '_blank', name: 'PHP'},
              {url: '#', target: '_blank', name: 'Web前端'}],
            second: [
              {name: '后端开发',
                datas: [
                  {url: '#', target: '_blank', name: '后端开发'},
                  {url: '#', target: '_blank', name: 'Java'},
                  {url: '#', target: '_blank', name: 'C++'},
                  {url: '#', target: '_blank', name: 'PHP'},
                  {url: '#', target: '_blank', name: 'C'},
                  {url: '#', target: '_blank', name: 'C#'},
                  {url: '#', target: '_blank', name: '.NET'},
                  {url: '#', target: '_blank', name: 'Hadoop'},
                  {url: '#', target: '_blank', name: 'Python'},
                  {url: '#', target: '_blank', name: 'Delphi'},
                  {url: '#', target: '_blank', name: 'VB'},
                  {url: '#', target: '_blank', name: 'Perl'},
                  {url: '#', target: '_blank', name: 'Ruby'},
                  {url: '#', target: '_blank', name: 'Node.js'},
                  {url: '#', target: '_blank', name: 'Golang'},
                  {url: '#', target: '_blank', name: 'Erlang'},
                  {url: '#', target: '_blank', name: '语音/视频/图形开发'},
                  {url: '#', target: '_blank', name: '数据采集'}
                ]
              },
              {name: '前端开发',
                datas: [
                  {url: '#', target: '_blank', name: '后端开发'},
                  {url: '#', target: '_blank', name: 'Java'},
                  {url: '#', target: '_blank', name: 'C++'},
                  {url: '#', target: '_blank', name: 'PHP'},
                  {url: '#', target: '_blank', name: 'C'},
                  {url: '#', target: '_blank', name: 'C#'},
                  {url: '#', target: '_blank', name: '.NET'},
                  {url: '#', target: '_blank', name: 'Hadoop'},
                  {url: '#', target: '_blank', name: 'Python'},
                  {url: '#', target: '_blank', name: 'Delphi'},
                  {url: '#', target: '_blank', name: 'VB'},
                  {url: '#', target: '_blank', name: 'Perl'},
                  {url: '#', target: '_blank', name: 'Ruby'},
                  {url: '#', target: '_blank', name: 'Node.js'},
                  {url: '#', target: '_blank', name: 'Golang'},
                  {url: '#', target: '_blank', name: 'Erlang'},
                  {url: '#', target: '_blank', name: '语音/视频/图形开发'},
                  {url: '#', target: '_blank', name: '数据采集'}
                ]
              }
            ]
          }
        ],
        right: {
          left: [{url: '#', target: '_blank', src: '/src/assets/img/1.jpg'}, {
            url: '#',
            target: '_blank',
            src: '/src/assets/img/2.jpg'
          }, {url: '#', target: '_blank', src: '/src/assets/img/3.jpg'}],
          right: [{url: '#', target: '_blank', src: '/src/assets/img/6.jpg'}, {
            url: '#',
            target: '_blank',
            src: '/src/assets/img/7.jpg'
          }, {url: '#', target: '_blank', src: '/src/assets/img/8.jpg'}]
        }
      }
    }
  }
</script>

<style scoped>

</style>
