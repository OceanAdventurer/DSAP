<template>
  <el-container>
    <el-header style="height: 45px;">
      <div class="nav-menu-header">
        <div class="nav-menu-header-left">
<!--          <img src="../../assets/images/logo_bg.png" alt="" :class="[logoIs]">-->
        </div>
        <div v-show="isCollapsed" class="logo-div"></div>
        <div :class="[rotateIcon, headerLogo]" @click="changeSlidebar()"></div>
        <div class="nav-menu-header-right">
          <div
          class="nav-menu-header-title nav-menu-header-title-active"
          >
            权限管理
          </div>
        </div>
      </div>
    </el-header>
    <el-container class="main-content">
      <el-menu  class="el-menu-vertical-msap"
        :collapse="!isCollapsed"
        >
      </el-menu>
      <el-main style="height:100%">
        <div class="chose-title" v-html="reminderTitle"></div>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data () {
    return {
      reminderTitle: '<b>您无权限访问此系统，请联系相应管理员开通。</b>',
      headerLogo: 'headerLogo', // 定义logo的样式
      systems: '',
      isCollapsed: true,
      pagesize: ''
    }
  },
  computed: {
    logoIs () { // 改变logo显示效果
      return [this.isCollapsed ? 'logo-sy' : 'logo-bg']
    },
    rotateIcon () { // 改变logo显示效果
      return ['menu-icon', this.isCollapsed ? 'rotate-icon' : '']
    }
  },
  methods: {
    changeSlidebar () {
      if (this.isCollapsed === true) {
        this.isCollapsed = false
      } else {
        this.isCollapsed = true
      }
    }
  }
}
</script>

<style>
.el-menu--popup{
  padding: 0px !important;
  border-radius: 4px !important;
}
.el-menu--collapse .el-menu .el-submenu, .el-menu--popup {
    min-width: 60px !important;
}

.el-menu--popup-right-start{
    margin-left: 12px !important;
  }
.el-menu-vertical-msap:not(.el-menu--collapse) {
  width: 180px;
  height: calc(100% - 15px);
  min-height: 300px;

  margin-bottom: 0;
  overflow: hidden;
  background-color: #2a436f;
  color: #fff;
}

.el-dropdown-menu__item--divided:before,
.el-menu,
.el-tooltip__popper.is-dark,
.el-menu--horizontal>.el-menu-item:not(.is-disabled):focus,
.el-menu--horizontal>.el-menu-item:not(.is-disabled):hover,
.el-menu--horizontal>.el-submenu .el-submenu__title:hover {
  background: #2a436f;
  color: #fff;
}

.el-tooltip__popper[x-placement^=right] .popper__arrow:after {
  border-right-color:  #2a436f;
}

.el-menu-item,
.el-submenu__title,
.el-submenu__title i,
.el-menu-item i,
.el-menu-item.is-active {
  color: #fff;
}

.el-menu-item.is-active,
.el-submenu__title:hover,
.el-menu-item:focus,
.el-menu-item:hover {
  background: rgba(255,255,255,0.10);
}
.el-menu-item,
.el-submenu__title {
  height: 44px;
  line-height: 44px;
}

.todo-list-li {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
}
.el-icon-menu:before {
  content: "";
  display: block;
  /*居中*/
  margin: 0 auto;
  /*自适应大小*/
  width: 1em;
  height: 1em;
  background-size: 100%!important;
}
/* 数据 */
.el-icon-data:before {
  background: url(../../assets/images/icon60.png) no-repeat center center;
}
/* 工程参数 */
.el-icon-paramOne:before {
  background: url(../../assets/images/icon61.png) no-repeat center center;
}
/* 分析参数 */
.el-icon-paramTwo:before {
  background: url(../../assets/images/icon62.png) no-repeat center center;
}
/* 事件 */
.el-icon-paramEvent:before {
  background: url(../../assets/images/icon62.png) no-repeat center center;
}
/* 分析 */
.el-icon-analysis:before {
  background: url(../../assets/images/icon63.png) no-repeat center center;
}
/* 数据挖掘 */
.el-icon-dataMining:before {
  background: url(../../assets/images/icon63.png) no-repeat center center;
}
/* APP */
.el-icon-app:before {
  background: url(../../assets/images/icon63.png) no-repeat center center;
}
/* 设置 */
/* .el-icon-setting:before {
  background: url(../../assets/images/icon63.png) no-repeat center center;
} */

/* 问题反馈 */
.el-icon-todoList:before {
  background: url(../../assets/images/icon_todo.png) no-repeat center center;
}
</style>
<style scoped>
.lg-p{
  position: relative;
}
.lg-s{
  position: absolute;
  bottom: 62px;
  left: 20px;
}
.chose-title{
  font-size: 30px;
  font-family: PingFang SC;
  font-weight: 500;
  line-height: 59px;
  color: #2A436F;
  opacity: 1;
  margin-top: 6%;
  margin-bottom: 4%;
  word-break: break-all;
  word-wrap: break-word;

}
.chose-name{
  font-size: 32px;
  font-family: PingFang SC;
  font-weight: 500;
  line-height: 45px;
  color: #FFFFFF;
  -webkit-text-stroke: 1 rgba(0, 0, 0, 0);
  opacity: 1;
}
.chose-enter{
  font-size: 16px;
  font-family: PingFang SC;
  font-weight: 500;
  line-height: 22px;
  color: #FFFFFF;
  -webkit-text-stroke: 1 rgba(0, 0, 0, 0);
  opacity: 1;
}
</style>
<style scoped>
.cm-popper__arrow{
  top: -6px;
  left: -12px;
  margin-right: 3px;
  border-top-width: 0;
  border-right-color: #2A436F;
  border-width: 6px;
  filter: drop-shadow(0 2px 12px rgba(0,0,0,.03));
  position: absolute;
  /* border-color: transparent; */
  border-top-color: transparent;
  border-left-color: transparent;
  border-bottom-color: transparent;
  border-style: solid;
}
.cm-popper__arrow::after{
  top: 1px;
  margin-left: -6px;
  border-top-width: 0;
  border-right-color: #2A436F;
  content: " ";
  border-width: 6px;
  position: absolute;
  display: block;
  border-color: transparent;
  border-style: solid;
}
.el-menu--popup-right-start{
  margin-left: 12px !important;
}
.logo-sy{
  margin-left: 20px;
}
.logo-bg{
  display: none;
}
/*头部*/
.el-header {
  position: absolute;
  width: 100%;
  top: 0;
  background-color: #2A436F;
  color: #FFFFFF;
  padding: 0 !important;
}
/*中间*/
.main-content {
  position: absolute;
  bottom: 0;
  width: 100%;
}
.logo-div {
  width: 117px;
  height: 35px;
}
/*中间左侧部分*/
.el-aside {
  height: 100%;
  float:left;
  margin-bottom: 0;
  overflow: auto;
  background-color: #2A436F;
  color: #FFFFFF;
  position: relative;
}

/*中间右侧部分*/
.el-main-default {
  height: 100%;
  padding: 0;
}

.el-main-content {
  height: 100%;
  margin-left: 0px;
}

/*左侧菜单*/
.nav-menu-left {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  justify-content: start;
}
.todo-list {
  position: absolute;
  bottom: 0;
  left: 0;

  width: 100%;
  height: 44px;
}

.nav-menu-item {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
  height: 44px;
  cursor: pointer;
}

.nav-menu-item-active {
  background: rgba(255,255,255,0.10);
}

.nav-menu-item > img {
  margin: 12px 18px;
  width: 20px;
  height: 20px;
}

.nav-menu-header {
  display: flex;
  flex-direction: row;
  align-items: flex-end;
  height:44px;
}

.nav-menu-header-left {
  display: flex;
  flex-direction: row;
  align-items: center;
}

/*头部左侧logo*/
.headerLogo {
  background-image:url(../../assets/images/icon1.png);
  background-size:cover;
  width: 35px;
  margin-right: 11px;
  cursor: pointer;
}

.menu-icon {
  transition: all 0.3s;
}

.rotate-icon {
  transform: rotate(-360deg);
}

/*头部菜单*/
.nav-menu-header-right {
  display: flex;
  flex-direction: row;
  align-items: flex-end;
}
.nav-menu-header-title {
  color: #fff;
  text-align: center;
  cursor: pointer;
}
.nav-menu-header-title-active {
  background: #F9F9F9;
  color: #3A6BB9;
}
.no-menu {
  display: flex;
  flex: 1;
  justify-content: center;
  align-items: center;
  position: absolute;
  height: 100%;
  width: 100%;
  font-size: 36px;
}
.logout {
  position: fixed;
  right: 20px;
  cursor: pointer;
}

/* 窗口高度大于800px */
@media screen and (min-height: 800px) {
  .el-header {
    height: 44px !important;
  }
  .nav-menu-header-left {
    width: 64px;
    height: 45px;
  }
  .logo-div {
    width: 116px;
  }
  .headerLogo {
    height: 45px;
    width: 35px;
  }
  .nav-menu-header-right {
    height: 40px;
  }
  .nav-menu-header-title {
    padding: 6px 20px;
    line-height: 22px;
    height: 22px;
  }
  .main-content {
    top: 44px;
  }
  .logout {
    top: 14px;
  }
}
/* 窗口高度小于800px */
@media screen and (max-height: 800px) {
  .el-header {
    height: 44px !important;
  }
  .nav-menu-header-left {
    width: 64px;
    height: 35px;
  }
  .headerLogo {
    height: 44px;
    width: 25px;
  }
  .nav-menu-header-right {
    height: 34px;
  }
  .nav-menu-header-title {
    padding: 6px 20px;
    line-height: 22px;
    height: 22px;
  }
  .main-content {
    top: 44px;
  }
  .logout {
    top: 14px;
  }
}

</style>
