<!--
 * @Author: your name
 * @Date: 2021-05-26 12:30:57
 * @LastEditTime: 2021-07-16 10:57:08
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /图谱最新预演代码/vue-d3-graph-main/src/components/gSearch.vue
-->
<template>
  <div style="margin-top: 20px;width: 500px;">
    <!-- <el-button style="margin-top: 15px;" @click="query">图数据切换，动态更新</el-button> -->
    <el-autocomplete style="width: 500px"
                     class="inline-input"
                     v-model="input"
                     :fetch-suggestions="querySearch"
                     placeholder="请输入内容"
                     :trigger-on-focus="false"
                     @select="handleSelect"
                     clearable>
      <!-- <el-select
        v-model="mode"
        slot="prepend"
        placeholder="关键字查询"
      >
        <el-option label="关键字查询" value="1"></el-option>
        <el-option label="单实体查询" value="2"></el-option>
        <el-option label="关联查询" value="3"></el-option>
      </el-select> -->
      <el-button slot="append"
                 type="success"
                 icon="el-icon-search"
                 @click="query">搜索</el-button>
    </el-autocomplete>
  </div>
</template>

<script>
export default {
  name: 'gSearch',
  // props: {
  //   isShowPrepend: {
  //     type: Boolean,
  //     default: true
  //   }
  // },
  data () {
    return {
      input: '',
      mode: '1',
      // 后台请求到的json数据
      // data: require('../data/records.json'),
      data: require('../data/new2dData.json'),
      results: []
    }
  },
  mounted () {
    this.$emit('getData', this.data)
    this.results = this.loadAll()
  },
  methods: {
    query () {
      // console.log(typeof this.mode)
      if (this.data.length >= 20) {
        this.data = require('../data/top5.json')
        console.log(this.data + '点击搜索出来的数据top5')
      } else {
        // this.data = require('../data/records.json')
        this.data = require('../data/new2dData.json')
        console.log(this.data + '点击搜索出来的数据records.json')
      }
      this.$emit('getData', this.data)
    },
    querySearch (queryString, cb) {
      var res = this.results
      var results = queryString ? res.filter(this.createFilter(queryString)) : res
      // 调用 callback 返回建议列表的数据
      cb(results)
    },
    createFilter (queryString) {
      return (res) => {
        return (res.value.toLowerCase().indexOf(queryString.toLowerCase()) !== -1)
      }
    },
    // 模拟加载数据
    loadAll () {
      return [
        { value: '节点100', address: '浙江诸暨艮塔路9号银证大厦8楼' },
        { value: '节点101', address: '玉环市芦浦镇漩门工业城' },
        { value: '节点102', address: '宁波市北仑区黄山西路235号' },
        { value: '节点103', address: '城西路4号' },
        { value: '节点104', address: '绍兴袍江启圣路以南与越英路交叉口生产车间' },
        { value: '节点105', address: '深圳' }
      ]
    },
    handleSelect (item) {
      console.log(item)
    }
  }
}
</script>

<style lang='scss' scoped>
.el-select {
  width: 120px;
  // background-color: #fff;
}
.input-with-select .el-input-group__prepend {
  background-color: #6ecbf3;
}
</style>
