<style scoped>
  .left_tree {
    overflow: auto;
    height: calc(100% - 41px);
  }
  .left_filter {
    margin-top: 5px;
    padding: 4px 10px 0px;
  }
.my-tree{
  width: 100%;height: 100%;
}
  .el-icon-search {
    font-size: 20px;
  }

  .left_tree::-webkit-scrollbar {
    width: 10px;
    height: 5px;
  }

  .left_tree::-webkit-scrollbar-thumb {
    border-radius: 10px;
    background-color: #cccccc;
  }
</style>
<template>
  <div class="my-tree">
    <!-- 过滤节点 -->
    <div class="left_filter" v-show="searchShow">
      <el-input placeholder="请输入关键字搜索" v-model="filterText" size="small" @keyup.enter.native="filterTree('treeKey')"
                v-if="isCurrentUser && !isLinkParam">
        <i slot="suffix" class="el-input__icon el-icon-search"
           @click="filterTree('treeKey')"></i>
      </el-input>
      <el-input placeholder="请输入关键字搜索" v-model="filterText" disabled="disabled" size="small"
                v-else>
        <i slot="suffix" class="el-input__icon el-icon-search"></i>
      </el-input>
    </div>
    <!-- 树 -->
    <div class="left_tree df-f1" v-loading="treeLoading">
      <el-tree :data="fileNewTreeArr"  node-key="ID" :props="defaultTreeProps" highlight-current
               :default-expanded-keys="expandedKeys"
               @node-click="handleNodeClick"  :render-content="renderContent"
               ref="tree" :filter-node-method="filterNode">
      </el-tree>
    </div>
    <div class="left_description" style="padding:20px 0px 20px 20px;width: 900px" v-show="showDescription">
      <span>说明：</span>
      <span class="descri" ref="descri_text"></span>
    </div>
  </div>
</template>
<script>
import TreeBase from 'components/base/TreeBase'

  export default {
    data () {
      return {
        parentId: '-1', // 筛选方式界面默认父节点id
        treeLoading: false,
        fileNewTreeArr: [],
        expandedKeys: [], // 默认要展开的keys
        defaultTreeProps: {
          label: 'NAME',
          children: 'CHILDREN'
        },
        iconMoreShow: false, // 自定义节点图标
        iconOtherShow: false, // 自定义节点图标
        iconShow: true, // 树节点过滤图标
        searchShow: true, // 树节点搜索框
        filterText: ''
      }
    },
    watch: {
      creatorId () {
        this.getTreeByCreatorId()
      },
      // 点击不同的按钮会触发一个 type，此处监听type如果发生改变，会调用方法
      treeType () {
        this.filterText = ''
        this.getTreeByCreatorId()
      },
      filterText (val) {
        if (val === '') { // 当清空值时查询
          this.filterTree()
        }
      }
    },
    props: ['creatorId', 'twoDimensionId', 'isLinkParam', 'isCurrentUser', 'treeType', 'showScript', 'showDescription'],
    created () {
    },
    mounted () {
      this.getTreeByCreatorId()
    },
    methods: {
      // 根据creatorId 和 treeType 查询不同的树
      getTreeByCreatorId () {
        var url = ''
        var searchParam = {}
        if (this.creatorId === 19 || this.creatorId === '19') { // 根据 测量值的 工程参数
          url = '/apm/getProfileCatalogByUserId'
          searchParam = {type: 3, jadmin: false, filter: true, searchContent: this.filterText}
        } else if (this.creatorId === 20 || this.creatorId === '20') { // 基于筛选凡是
          if (this.treeType === 'shaixuanway') { // 筛选方式
            url = '/apm/getProfileCatalogByUserId'
            searchParam = {type: 2, jadmin: false, filter: true, searchContent: this.filterText}
          } else if (this.treeType === 'gongchengparam') { // 工程参数
            searchParam = {parentId: this.parentId, content: this.filterText}
            url = '/apm/getGpTree'
          }
        } else if (this.creatorId === 21 || this.creatorId === '21') { // // 基于filter  工程参数
          searchParam = {parentId: this.parentId, content: this.filterText}
          url = '/apm/getGpTree'
        } else { // 筛选 多个工程参数判断
          searchParam = {parentId: this.parentId, content: this.filterText}
          url = '/apm/getGpTree'
        }
        if (url) {
          this.getTree(url, searchParam)
        }
      },
      // 初始化树
      getTree (url, param) {
        this.$bus.$emit('updateShowDescription', false)
        if (this.isCurrentUser && !this.isLinkParam) { // 有权限才执行
          this.treeLoading = true
          this.$axios.get(url, {params: param}).then(response => {
            this.treeLoading = false
            var data = response.data
              if (data && data.status && data.status === 'E1001') {
                this.$router.push({path: '/'})
              } else {
                if (url === '/apm/getGpTree') {
                  this.fileNewTreeArr = data.data
                  if (data.defaultNode) {
                    this.expandedKeys = data.defaultNode
                  }
                } else {
                  this.fileNewTreeArr = data
                }
              }
          }).catch(response => {
            this.$message.error('数据加载失败!')
            this.treeLoading = false
          })
        }
      },
      // 树节点过滤图标
      filterTree () {
        this.getTreeByCreatorId()
      },
      // 刷新事件
      refresh () {
        this.treeLoading = true
        this.getTree()
      },
      filterNode (value, data) {
        if (!value) return true
        return data.NAME.indexOf(value) !== -1
      },
      // 自定义子节点内容
      renderContent (h, {node, data, store}) {
        return h(TreeBase, {
          props: {
            data: data, // 当前节点的数据
            node: node // 当前节点的Node对象
          },
          on: {
          }
        })
      },
      // 点击子节点击事件 加载中间及右侧数据
      handleNodeClick (data, node, nodeCommpent) {
        this.$bus.$emit('updateShowDescription', false)
        if (data.NAME !== null) {
          if (data['CHILDREN']) { // 点击父节点
            this.$emit('getTreenodeData1', data)
          } else {
            if (data.DESCRIPTION) {
              this.$refs.descri_text.textContent = data.DESCRIPTION
              this.$bus.$emit('updateShowDescription', true)
            } else {
              this.$bus.$emit('updateShowDescription', false)
            }
            this.$emit('getTreenodeData1', data)
          }
        }
      }
    }
  }
</script>
