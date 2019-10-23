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
            <div class="sub-li" >
              <a :href="'javascript:void(1)'" @click="join2project(item.data.proInfoId)" class="job-info" >
                <p>{{item.data && item.data.proName}}<span class="salary">{{item.data && item.data.proOutlay}}</span></p>
                <p class="job-text">{{item.data && item.data.proOrigin}}<span class="vline"></span>{{item.data && item.data.proIntroduce}}</p>
              </a>
              <!-- 企业 -->
              <a v-if="item.data.entEnterpriseInfo && false" @click="join2project(item.data.proInfoId)" :href="'javascript:void(1)'" class="user-info" >
                <p>
                  <img :src="item.data.entEnterpriseInfo.entLogo" :alt="'企业Logo'" title="企业Logo">{{item.data.entEnterpriseInfo.entName}} -
                  <span class="user-text">{{item.data.entEnterpriseInfo.entCorporate}}<span class="vline"></span>{{item.data.entEnterpriseInfo.entType}}</span>
                </p>
              </a>
              <!-- 学生 -->
              <a v-if="item.data.userPersonInfo && false" @click="join2project(item.data.proInfoId)" :href="'javascript:void(1)'" class="user-info" >
                <p>
                  <span class="user-text">{{item.data.userPersonInfo.sysUserEntity.name}}<span class="vline"></span>{{item.data.userPersonInfo.perSchoolPost}}</span>
                </p>
              </a>
              <!-- 教师 -->
              <a v-if="item.data.userTeacherInfo && false" @click="join2project(item.data.proInfoId)" :href="'javascript:void(1)'" class="user-info" >
                <p>
                  <span class="user-text">{{item.data.userTeacherInfo.sysUserEntity.name}}<span class="vline"></span>{{ item.data.userTeacherInfo.teacherPost}}</span>
                </p>
              </a>
              <a class="user-info" @click="join2project(item.data.proInfoId)" href="javascript:void(1)">
                <p><sapn class="user-text">点击查看项目</sapn></p>
              </a>
            </div>
          </li>
        </ul>
      </div>
      <p class="common-tab-more"  v-if="more"><a class="btn btn-outline" :href="more.url" :target="more.target || '_blank'">{{more.name}}</a></p>
    </div>
    <!-- 详情 -->
    <project-join v-if="xiangqing" ref="pj"/>
  </div>
</template>

<script>
  // 招聘信息
  import ProjectJoin from './project-join'
  export default {
    props: {hide: Boolean},
    data () {
      return {
        xiangqing: false,
        list: null,
        menu: [{name: 'IT·互联网', cur: true}, {name: '金融', cur: false}, {name: '房地产·建筑', cur: false}, {name: '教育培训', cur: false}, {name: '汽车', cur: false},
          {name: '娱乐传媒', cur: false}, {name: '医疗健康', cur: false}, {name: '法律咨询', cur: false}, {name: '供应链·物流', cur: false}, {name: '采购贸易', cur: false}],
        more: {name: '查看更多', url: '#'},
        pageIndex: 1,
        pageSize: 12,
        hasApply: '1'
      }
    },
    components: {
      ProjectJoin
    },
    mounted () {
      this.getListInfo(0, 0)
    },
    methods: {
      // 加入项目详情
      join2project: function (id) {
        console.log(this.$cookie.get('token'))
        if (!this.$cookie.get('token')) {
          this.$message.warning('您还未登入,不能查看加入项目')
          return
        }
        this.xiangqing = true
        this.$nextTick(() => {
          this.$refs.pj.init('加入项目详情', id)
        })
      },
      // 处理数据格式
      invokeDatas (data) {
        return {
          url: '#',
          target: '_blank',
          data: data
        }
      },
      getPeojectInfos (proType) {
        let nu = parseInt(proType) + 1
        this.$http({
          url: this.$http.adornUrl('/webpage/projectInfos'),
          method: 'get',
          params: this.$http.adornParams({
            'currPage': this.pageIndex,
            'pageSize': this.pageSize,
            'inApply': this.hasApply,
            'proType': nu
          })
        }).then(({data}) => {
          console.log(data)
          if (data.code === 500) {
            this.$message.error(data.msg)
          } else {
            if (data.data) {
              let that = this
              const result = []
              data.data.records && data.data.records.forEach(function (item, index, arr) {
                result.push(that.invokeDatas(item))
              })
              this.list = result
              console.log('list', this.list)
            }
          }
        })
      },
      getListInfo (type, index) {
        console.log(type, index)
        let that = this
        this.menu.forEach((item, aindex, arr) => {
          // console.log(item, aindex, arr)
          item['cur'] = false
          if (index === aindex) {
            that.menu[aindex].cur = true
            // TODO请求网络数据，回填list
            that.getPeojectInfos(index)
          }
        })
      }
    }
  }
</script>

<style scoped>

</style>
