<template>
  <div class="app-wrap" v-if="!hide" style="margin-top: 20px;">
    <div class="app-w app-menu clearfix">
      <!-- 左边 -->
      <div class="app-menu-left fl-l" v-if="false">
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
      <div class="app-menu-right clearfix"  style="margin-left: 0;">
        <ul class="fl-l app-menu-rleft" style="width: 620px;">
          <li v-if="right && right.left" v-for="(item, index) in right.left" :key="index"><a :href="item.url" :target="item.target || '_blank'"><img :src="item.src" :alt="item.src" :title="item.src"></a></li>
        </ul>
        <div class="app-menu-rright" style="margin-left: 630px;">
          <div class="gg-box">
            <h3 class="gg-title">通知公告</h3>
            <div class="gg-content" style="height: 160px;">
              <p class="gg-item"><a href="#" class="gg-text">习近平告诉你 我们制度优势如何成为治理体系优势习近平告诉你 我们制度优势如何成为治理体系优势</a></p>
              <p class="gg-item"><a href="#" class="gg-text">政治局集体学习专议这项技术发展 习近平有何深远考量政治局集体学习专议这项技术发展 习近平有何深远考量</a></p>
              <p class="gg-item"><a href="#" class="gg-text">系统集成 协同高效 改革协调推进系统集成 协同高效 改革协调推进</a></p>
              <p class="gg-item"><a href="#" class="gg-text">第二届进博会11月5日开幕 凸显五大亮点第二届进博会11月5日开幕 凸显五大亮点</a></p>
              <p class="gg-item"><a href="#" class="gg-text">人民日报钟声：相约进博会 书写新篇章人民日报钟声：相约进博会 书写新篇章</a></p>
              <p class="gg-item"><a href="#" class="gg-text">习近平告诉你 我们制度优势如何成为治理体系优势习近平告诉你 我们制度优势如何成为治理体系优势</a></p>
              <p class="gg-item"><a href="#" class="gg-text">政治局集体学习专议这项技术发展 习近平有何深远考量政治局集体学习专议这项技术发展 习近平有何深远考量</a></p>
              <p class="gg-item"><a href="#" class="gg-text">系统集成 协同高效 改革协调推进系统集成 协同高效 改革协调推进</a></p>
              <p class="gg-item"><a href="#" class="gg-text">第二届进博会11月5日开幕 凸显五大亮点第二届进博会11月5日开幕 凸显五大亮点</a></p>
              <p class="gg-item"><a href="#" class="gg-text">人民日报钟声：相约进博会 书写新篇章人民日报钟声：相约进博会 书写新篇章</a></p>
            </div>
          </div>
          <div class="gg-box" style="height: 295px;margin-top: 13px;">
            <h3 class="gg-title">最新项目发布</h3>
            <div class="gg-content" style="height: 280px">
              <ul class="gg-pc" :style="'margin-top:' + -top + 'px'">
                <li class="gg-pro" v-for="i in da">
                  <p class="gg-textl">
                    <a href="#" class="gg-text">创新区块链项目</a>
                  </p>
                  <p class="gg-textl">
                    <span class="gg-text">来源灵感</span>&nbsp;<span class="gg-text">|</span>
                    <span class="gg-text">上个大学一个APP还不够</span>
                  </p>
                  <p class="gg-textl">
                    <span class="gg-text">点击查看项目</span>
                  </p>
                </li>
              </ul>
            </div>
          </div>
        </div>
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
        top: 0,
        stepN: 1,
        height: 125,
        max: 0,
        da: [1, 2, 3, 1, 2, 3],
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
    },
    mounted () {
      this.scrollTip()
    },
    methods: {
      scrollTip () {
        this.da.concat(this.da)
        this.max = this.height * (this.da.length / 2 - 1)
        this.play()
      },
      play () {
        let that = this
        setInterval(function () {
          console.log(that.top >= that.max)
          if (that.top >= that.max + 60) {
            that.top = 0
          }
          that.top = that.top + that.stepN
        }, 30)
      }
    }
  }
</script>
<style>
  @import "../../../../static/plugins/scroll/scroll.css";
</style>
<style scoped>
  .gg-box{max-width: 540px;padding:10px;background-color: #fff;border-radius: 5px;box-shadow: 0 0 10px 0px #e8e5e5;}
  .gg-title{font-weight: normal;color: #5dd5c8;font-size: 20px;line-height: 39px;border-bottom: 1px solid #e8e5e5;}
  .gg-content{height: 200px;overflow: hidden;}
  .gg-item,.gg-textl{line-height: 30px;height: 30px;margin-top: 1px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap;border-bottom: 1px dashed #e8e5e5;}
  .gg-text{color: #4b4b4b;font-size: 16px;transition: .5s;}
  .gg-item:hover{background-color: #eee;}
  .gg-text:hover{margin-left: 5px;color: #00d7c6;}
  .gg-pc{padding: 0 10px;}
  .gg-pro{height: 99px;padding:8px;box-shadow: 0 0 10px 0px #e8e5e5;margin-top:10px;border-radius: 8px;cursor: pointer;}
  .gg-pro:hover{background-color: #f3f3f3;}
  .gg-textl:hover .gg-text{color: #00d7c6;}
</style>
