<template>
  <div class="bookmarks">
    <h1 class="tip">持续更新...</h1>
    <div class="bookmarks-wrapper">
      <div class="left-wrapper">
        <button @click="toggleNav" href="javascript:;" class="nav-btn" :class="{'open':isNavShow}">
          <span class="icon-nav"></span>
          <span class="icon-nav"></span>
          <span class="icon-nav"></span>
        </button>
        <div class="left-nav" :class="{'show':isNavShow}">
          <div class="nav-wrapper" ref="navWrapper">
            <ul>
              <li class="top-item">
                <a href="javascript:;" @click="updateList(-1, -1)" class="top-name" :class="{'active':(curTopIndex===-1 && curSubIndex===-1)}"><i class="icon icon-zh_strong"></i>综合推荐</a>
              </li>
              <li class="top-item" v-for="(bookmark, topindex) in bookmarks" :key="topindex">
                <a href="javascript:;" @click="updateList(bookmark.type, -1)" class="top-name" :class="{'active':(curTopIndex===topindex && curSubIndex===-1)}"><i class="icon" :class="bookmark.icon"></i>{{bookmark.name}}</a>
                <ul class="sub-list" v-if="bookmark.sublist || bookmark.sublist.length>0">
                  <li class="sub-item" v-for="(sub, subindex) in bookmark.sublist" :key="subindex">
                    <a href="javascript:;" @click="updateList(bookmark.type, sub.type)" class="sub-name" :class="{'active':(topindex===curTopIndex && subindex===curSubIndex)}">{{sub.name}}</a>
                  </li>
                </ul>
              </li>
            </ul>
          </div>
        </div>
        <div @click="_closeNav" class="nav-bg" :class="{'show':isNavShow}"></div>
      </div>
      <div class="content-wrapper clearfix">
        <a class="content-item" v-for="(item, index) in curList" :key="index" :href="item.url" target="_blank">
          <div class="top-wrapper">
            <span class="pic"><img width="40" height="40" :src="item.pic && item.pic !== ''?item.pic:defaultPic" onerror="this.onerror='';src='./static/imgs/default.png'"></span>
            <div class="title">
              <span class="name" :title="item.name">{{item.name}}</span>
              <span class="url" :title="item.url">{{item.url}}</span>
            </div>
          </div>
          <div class="description" :title="item.description">{{item.description}}</div>
        </a>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  export default {
    data() {
      return {
        curTopIndex: -1,
        curSubIndex: -1,
        defaultPic: './static/imgs/default.png',
        isNavShow: false,
        bookmarks: [
          {
            name: '前端相关',
            type: 0,
            icon: 'icon-code_strong',
            sublist: [
              {
                name: '开发工具',
                type: 0,
                items: [
                  {
                    name: 'Bootstrap中文网',
                    pic: './static/imgs/bootstrap.png',
                    description: '提供了优质技术文档和辅助开发工具。',
                    url: 'http://www.bootcss.com/'
                  },
                  {
                    name: 'NEC.netease',
                    pic: './static/imgs/nec.png',
                    description: '提供了一套框架、代码库和编程规范。',
                    url: 'http://nec.netease.com'
                  },
                  {
                    name: 'Zan UI',
                    pic: './static/imgs/zanui.png',
                    description: '好看、好用的组件库，移动、PC、小程序，该有的都有了。',
                    url: 'https://www.youzanyun.com/zanui'
                  },
                  {
                    name: 'IconFont-阿里巴巴矢量图标库',
                    pic: './static/imgs/iconfont.png',
                    description: '设计师将图标上传到Iconfont平台，用户可以自定义下载多种格式的icon，平台也可将图标转换为字体，便于前端工程师自由调整与调用。',
                    url: 'http://iconfont.cn/'
                  },
                  {
                    name: 'Icomoom',
                    pic: './static/imgs/icomoom.png',
                    description: '可以选择或上传SVG图标到icomoom生成字体文件，供前端开发调用。',
                    url: 'http://icommom.io/'
                  },
                  {
                    name: 'Bootstrap文字图标',
                    pic: './static/imgs/bsicon.png',
                    description: '快速查看Bootstrap文字图标代码。',
                    url: 'http://www.runoob.com/try/demo_source/bootstrap-glyph-customization.htm'
                  },
                  {
                    name: '贝塞尔曲线取值',
                    pic: './static/imgs/bezier.png',
                    description: 'This curve contains values out of range. But fear not young padawan! Just use cubic-bezier(.17,.67,.73,0) as well for Webkit until the bug #45761 fix propagates to Safari.',
                    url: 'http://cubic-bezier.com'
                  }
                ]
              },
              {
                name: '成长之路',
                type: 1,
                items: [
                  {
                    name: 'FreeCodeCamp',
                    pic: './static/imgs/freecodecamp.png',
                    description: 'Learn to code for free.',
                    url: 'https://www.freecodecamp.org/'
                  },
                  {
                    name: 'CodeCademy',
                    pic: './static/imgs/codecademy.png',
                    description: '在线学习编程，一步一步完成指定任务。',
                    url: 'https://www.codecademy.com/'
                  },
                  {
                    name: 'Flexbox Froggy',
                    pic: './static/imgs/flexbox.png',
                    description: '通过游戏的方式学习/练习flex布局。',
                    url: 'http://flexboxfroggy.com/'
                  },
                  {
                    name: '极客学院IT在线教育',
                    pic: './static/imgs/jikexueyuan.png',
                    description: '通过实战学习模式为注重自我持续提升的IT从业者提供高质量的职业教育。',
                    url: 'http://www.jikexueyuan.com/'
                  },
                  {
                    name: '慕课网-程序员的梦工厂',
                    pic: './static/imgs/imooc.png',
                    description: '提供海量优质课程。',
                    url: 'https://www.imooc.com/'
                  },
                  {
                    name: '京程一灯-腾讯视频',
                    pic: './static/imgs/qqvideos.png',
                    description: '跟前端大牛学前端，学吹逼。',
                    url: 'http://v.qq.com/vplus/81fd9e07132a6926b87401b4530cff9a/videos'
                  }
                ]
              },
              {
                name: '知名博客',
                type: 2,
                items: [
                  {
                    name: 'Vinst Blog',
                    pic: './static/imgs/vinst.png',
                    description: '本站作者的个人博客。',
                    url: 'http://www.hansmkiii.com/'
                  },
                  {
                    name: '阮一峰个人博客',
                    pic: './static/imgs/ruanyifeng.png',
                    description: 'IT技术博客，程序员必看。',
                    url: 'http://www.ruanyifeng.com/blog/'
                  },
                  {
                    name: '廖学锋的官方网站',
                    pic: '',
                    description: '小白学习 JavaScript、Python、Git 的好去处。',
                    url: 'https://www.liaoxuefeng.com/'
                  },
                  {
                    name: 'Smashing Magazine',
                    pic: './static/imgs/smashing.png',
                    description: '写给网页设计和开发者的英文博客。',
                    url: 'https://www.smashingmagazine.com/'
                  }
                ]
              },
              {
                name: '社区',
                type: 3,
                items: [
                  {
                    name: 'Stack Overflow',
                    pic: './static/imgs/stackoverflow.png',
                    description: '有问题？去 Stack Overflow，全球最优的程序员社区之一。',
                    url: 'https://stackoverflow.com/'
                  },
                  {
                    name: 'SegmentFault',
                    pic: './static/imgs/segmentfault.png',
                    description: '中国版的Stack Overflow',
                    url: 'https://segmentfault.com/'
                  },
                  {
                    name: '掘金',
                    pic: './static/imgs/juejin.png',
                    description: '一个帮助开发者成长的社区',
                    url: 'https://juejin.im/'
                  },
                  {
                    name: '前端网(QDfuns)',
                    pic: './static/imgs/qdfuns.png',
                    description: '前端文章创作、交流心得体会。',
                    url: 'https://www.qdfuns.com'
                  },
                  {
                    name: 'Div.IO',
                    pic: './static/imgs/divio.png',
                    description: '高质量前端资源汇聚。',
                    url: 'http://div.io'
                  },
                  {
                    name: '大前端',
                    pic: './static/imgs/daqianduan.png',
                    description: '关注前端开发，关注用户体验。',
                    url: 'http://www.daqianduan.com/'
                  },
                  {
                    name: '鱼C工作室',
                    pic: './static/imgs/fishc.png',
                    description: '一个关注了很多年的社区，从当初的汇编语言、C++，到现在发展越来越壮大。',
                    url: 'http://bbs.fishc.com/forum.php'
                  }
                ]
              },
              {
                name: '面经',
                type: 4,
                items: [
                  {
                    name: '前端面试题集锦',
                    'pic': './static/imgs/github.png',
                    description: '前端笔试题、面试题集锦，作者持续更新。',
                    url: 'https://fe.padding.me/#/questions/'
                  }
                ]
              }
            ]
          },
          {
            name: '设计相关',
            type: 1,
            icon: 'icon-design_strong',
            sublist: [
              {
                name: '设计大观',
                type: 0,
                items: [
                  {
                    name: '站酷网',
                    pic: './static/imgs/zcool.png',
                    description: '深耕设计领域十年，站酷聚集了500万+优秀设计师、摄影师、插画师、艺术家、创意人，每日会员发布原创作品20000幅，产生互动交流200000次，覆盖中国300+城市，在设计创意群体中具有强大的影响力与号召力。',
                    url: 'http://www.zcool.com.cn'
                  },
                  {
                    name: 'Behance',
                    pic: './static/imgs/behance.png',
                    description: '设计在线作品集，展示和发现创意作品。',
                    url: 'https://www.behance.net/'
                  },
                  {
                    name: 'Dribbble',
                    pic: './static/imgs/dribbble.png',
                    description: 'Dribbble is where designers get inspired and hired.',
                    url: 'https://dribbble.com/'
                  },
                  {
                    name: '优设网',
                    pic: './static/imgs/uisdc.png',
                    description: '大量的设计文章、教程，都是干货。',
                    url: 'http://www.uisdc.com/'
                  },
                  {
                    name: '花瓣网',
                    pic: './static/imgs/huaban.png',
                    description: '花瓣，陪你做生活的设计师，发现、采集你喜欢的设计。',
                    url: 'http://huaban.com/'
                  },
                  {
                    name: 'Collect UI',
                    pic: './static/imgs/collectui.png',
                    description: '每天更新，收集各种UI作品，寻找创意灵感。。',
                    url: 'https://collectui.com'
                  },
                  {
                    name: 'Doyoudo',
                    pic: './static/imgs/doyoudo.png',
                    description: '图片、视频特效制作教程，手把手教你做大片。',
                    url: 'http://doyoudo.com/'
                  }
                ]
              },
              {
                name: '网站配色',
                type: 1,
                items: [
                  {
                    name: 'Flat UI Colors',
                    pic: './static/imgs/flatuicolors.png',
                    description: '14款配色推荐。',
                    url: 'https://flatuicolors.com/'
                  }
                ]
              },
              {
                name: '图标',
                type: 2,
                items: [
                  {
                    name: 'IconFont-阿里巴巴矢量图标库',
                    pic: './static/imgs/iconfont.png',
                    description: '设计师将图标上传到Iconfont平台，用户可以自定义下载多种格式的icon，平台也可将图标转换为字体，便于前端工程师自由调整与调用。',
                    url: 'http://iconfont.cn/'
                  },
                  {
                    name: 'Icomoom',
                    pic: './static/imgs/icomoom.png',
                    description: '可以选择或上传SVG图标到icomoom生成字体文件，供前端开发调用。',
                    url: 'http://icommom.io/'
                  }
                ]
              },
              {
                name: '高清图库',
                type: 3,
                items: [
                  {
                    name: 'The Stocks',
                    pic: './static/imgs/thestocks.png',
                    description: 'The Stocks is an handpicked curation of the best stock photos, videos, audios, fonts, icons and mockup websites.',
                    url: 'http://thestocks.im/'
                  },
                  {
                    name: 'Split Shire',
                    pic: './static/imgs/splitshire.png',
                    description: 'Free Stock Photos and Videos for commercial use.',
                    url: 'http://splitshire.im/'
                  },
                  {
                    name: 'Pexels',
                    pic: './static/imgs/pexels.png',
                    description: 'Best free stock photos in one place.',
                    url: 'https://www.pexels.com/'
                  }
                ]
              }
            ]
          }
        ]
      }
    },
    computed: {
      curList() {
        let curArr = []
        if (this.curTopIndex === -1) {
          this.bookmarks.forEach((topli) => {
            topli.sublist.forEach((subli) => {
              subli.items.forEach((item) => {
                if (!curArr.includes(item)) {
                  curArr.push(item)
                }
              })
            })
          })
        } else if (this.curSubIndex === -1) {
          this.bookmarks[this.curTopIndex].sublist.forEach((subli) => {
            subli.items.forEach((item) => {
              if (!curArr.includes(item)) {
                curArr.push(item)
              }
            })
          })
        } else {
          this.bookmarks[this.curTopIndex].sublist[this.curSubIndex].items.forEach((item) => {
            if (!curArr.includes(item)) {
                curArr.push(item)
              }
          })
        }
        return curArr
      }
    },
    methods: {
      _initScroll() {
        this.navScroll = new BScroll(this.$refs.navWrapper, {
          click: true
        })
      },
      _closeNav() {
        this.isNavShow = false
      },
      updateList(toptype, subtype) {
        this.curTopIndex = toptype
        this.curSubIndex = subtype
        this._closeNav()
      },
      toggleNav() {
        this.isNavShow = !this.isNavShow
        if (this.isNavShow) {
          this._initScroll()
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
body
  background: #eee
  .bookmarks
    position: relative
    padding-top: 50px
    .tip
      position: absolute
      top:0
      left:0
      margin: 0
      width:100%
      line-height: 50px
      text-align: center
      font-size: 16px
      font-weight: 700
      color: #dadada
    .bookmarks-wrapper
      /* display: flex */
      position: relative
      width: 80%
      max-width: 1040px
      margin: 0px auto
      .left-wrapper
        position: absolute
        z-index: 10
        left:0
        top:0
        width: 120px
        border-right: 1px solid rgba(7, 17, 27, 0.1)
        transition: all 0.2s
        .nav-btn
          display: none
        .left-nav
          display: block
          .top-item
            margin-bottom: 10px
            .top-name
              display: block
              height: 16px
              line-height: 16px
              margin-bottom: 12px
              font-size: 16px
              white-space: nowrap
              overflow: hidden
              text-overflow: ellipsis
              &.active
                color: #0085a1
                font-weight: 700
              .icon
                display: inline-block
                vertical-align: top
                margin-right: 5px
                height: 16px
                line-height: 16px
            .sub-list
              padding-left: 30px
              .sub-item
                font-size: 0
                .sub-name
                  display: block
                  height: 14px
                  line-height: 14px
                  margin-bottom: 12px
                  font-size: 14px
                  white-space: nowrap
                  overflow: hidden
                  text-overflow: ellipsis
                  &.active
                    color: #0085a1
                    font-weight: 700
        .nav-bg
          display: none
      @media screen and (max-width: 630px)
        .left-wrapper
          z-index: auto
          margin-left: -10%
          margin-top:-40px
          width: 40px
          height: 40px
          border: none
          .nav-btn
            position: relative
            z-index: 2100
            display: block
            width: 40px
            height: 40px
            padding: 11px
            border: none
            border-radius: 50%
            background: none
            outline: none
            cursor: pointer
            transition: all 0.2s
            &:focus
              outline: none
            .icon-nav
              display: block
              width: 18px
              height: 2px
              margin-top: 3px
              background: #333
              &:first-child
                margin-top: 0
            &.open
              background: #fff
          .left-nav
            position: fixed
            z-index: 2100
            top: 50%
            left: 60px
            padding: 20px
            width: 50%
            height: 80%
            box-sizing: content-box
            background: #fff
            border-radius: 5px
            box-shadow: 3px 4.33px 5px 0px rgba( 7, 17, 27, 0.1 )
            overflow: hidden
            transition: all 0.4s
            transform: translate3d(0, -170%, 0)
            opacity: 0
            &.show
              transform: translate3d(0, -50%, 0)
              opacity: 1
            .nav-wrapper
              height: 100%
          .nav-bg
            position: fixed
            display: block
            left: 0
            top: 0
            z-index: -1
            width: 100%
            height: 100%
            opacity: 0
            transition: all 0.4s
            &.show
              opacity: 1
              z-index: 2000
              background: rgba(7, 17, 27, 0.3)
              backdrop-filter: blur(10px)
      .content-wrapper
        position: relative
        width: 100%
        box-sizing: border-box
        padding-left: 160px
        font-size: 0
        .content-item
          float:left
          width: 30%
          min-width:110px
          margin: 0 1.5% 12px 1.5%
          height: 110px
          display: inline-block
          box-sizing: border-box
          padding: 13px 15px 15px 15px
          overflow: hidden
          background: #f6f6f6
          border-radius: 6px
          transition: all 0.2s
          box-shadow: 0px 4.33px 5px 0px rgba( 111, 111, 111, 0.1 )
          &:hover
            box-shadow: 0px 4.33px 5px 0px rgba( 1, 1, 1, 0.25 )
          .top-wrapper
            position: relative
            height: 40px
            .pic
              position: absolute
              display: block
              left: 0
              top: 0
              width: 40px
              height: 40px
              border-radius: 50%
            .title
              display: block
              padding-top: 1px
              margin-left: 50px
              font-size: 0
              .name
                display: block
                width:100%
                height: 14px
                line-height: 14px
                margin-top: 8px
                overflow: hidden
                text-overflow: ellipsis
                white-space: nowrap
                font-size: 14px
                font-weight: 700
                color: #46474f
              .url
                display: block
                width:100%
                height: 14px
                line-height: 14px
                overflow: hidden
                text-overflow: ellipsis
                white-space: nowrap
                font-size: 12px
                color: #a4a4a4
          .description
            display: -webkit-box;
            overflow: hidden
            height: 32px
            line-height: 16px
            -webkit-box-orient: vertical;
            -webkit-line-clamp: 2;
            margin-top: 10px
            padding-left: 5px
            font-size: 12px
            color: #8c8d8f
        @media screen and (max-width: 880px)
          .content-item
            width: 48%
            margin: 0 1% 12px 1%
        @media screen and (max-width: 400px)
          .content-item
            width: 100%
            margin: 0 0 12px 0
      @media screen and (max-width: 630px)
        .content-wrapper
          padding-left: 0
</style>
