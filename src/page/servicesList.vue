/***       配套服务        ***/
<template>
  <div id="servicesList">
    <div class="breadcrumb">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/home' }">{{language.home}}</el-breadcrumb-item>
        <el-breadcrumb-item>配套服务</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="servicesList-Cont">
      <div class="SEO clear">
        <div class="Left">
          <el-form :inline="true" class="demo-form-inline">
            <el-form-item>
              <el-input v-model="SEO" size="small"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" size="small" @click="SEOFn">搜索</el-button>
            </el-form-item>
          </el-form>
        </div>
        <div class="Right">
          <el-button type="success" size="small" @click="operationFn(0)">添加</el-button>
          <el-button type="primary" size="small" @click="operationFn(1)">编辑</el-button>
          <el-button type="danger" size="small" @click="operationFn(2)">删除</el-button>
          <el-button type="" size="small" @click="operationFn(3)">上架</el-button>
          <el-button type="" size="small" @click="operationFn(4)">下架</el-button>
        </div>
      </div>
      <div class="Table">
        <el-table
          ref="multipleTable"
          v-loading="loading"
          :data="SupportServicesList"
          tooltip-effect="dark"
          style="width: 100%"
          @selection-change="selectionFn">
          <el-table-column
            type="selection"
            fixed
            width="55">
          </el-table-column>
          <el-table-column
            prop="name"
            label="服务名称">
          </el-table-column>
          <el-table-column
            prop="unitPrice"
            label="价格(元)">
          </el-table-column>
          <el-table-column
            prop="type"
            label="付费方式"
            show-overflow-tooltip>
            <template slot-scope="scope">
              {{scope.row.type === 1 ? '一次性付费' : (scope.row.type === 2 ? '充值型' : (scope.row.type === 3 ? '按年付费' : ''))}}
            </template>
          </el-table-column>
          <el-table-column
            prop="state"
            label="状态"
            show-overflow-tooltip>
          </el-table-column>
          <el-table-column
            prop="profile"
            label="描述"
            show-overflow-tooltip>
          </el-table-column>
        </el-table>
      </div>
      <div class="pagination clear">
        <div class="Left">
          <el-pagination
            @size-change="SizeChangeFn"
            @current-change="CurrentChangeFn"
            :current-page="page"
            :page-sizes="[20, 40, 60, 80]"
            :page-size="pageNum"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total">
          </el-pagination>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    components: {},
    name: 'servicesList',
    computed: {
      languageType: function () {
        return this.$store.getters.languageType
      },
      language () {
        let that = this
        let val = that.languageType
        if (val) {
          return val
        }
        return
      }
    },
    data () {
      return {
        SEO: '', // 搜索
        total: 0, // 总数
        SupportServicesList: [], // 数据
        page: 1, // 页数
        loading: false, // 每页数量
        checkRows: [], // 选择行
        pageNum: 20 // 每页数量
      }
    },
    watch: {},
    methods: {
      SEOFn: function () {  // 搜索
        this.getSupportServicesFn()
      },
      getSupportServicesFn: function () { // 查询配套服务
        let that = this
        that.loading = true
        that.$store.dispatch('getSupportServices', {
          name: that.SEO, // 配套名称
          offset: that.page, // 页数
          size: that.pageNum, // 每页条数
          fn (data) {
            that.loading = false
            if (data.code === 0) {
              let Data = data.data
              for (let i = 0; i < Data.length; i++) {
                Data[i].state = Data[i].state + '' === '1' ? '上架' : '下架'
              }
              that.total = data.count
              console.log(Data)
              that.SupportServicesList = data.data
            } else {
              that.$message.error(data.msg)
            }
          }
        })
      },
      removeSupportServicesFn: function () { // 删除
        let that = this
        let checkRows = that.checkRows
        let ids = []
        for (let i = 0; i < checkRows.length; i++) {
          ids.push(checkRows[i].id)
        }
        that.$store.dispatch('removeSupportServices', {
          ids: ids.join(','),
          fn (data) {
            if (data.code === 0) {
              that.$message({
                type: 'success',
                message: '删除成功！'
              })
              that.getSupportServicesFn()
            } else {
              that.$message.error(data.msg)
            }
          }
        })
      },
      upDownSupportServicesFn: function (state) { // 批量上下架
        let that = this
        let checkRows = that.checkRows
        let ids = []
        for (let i = 0; i < checkRows.length; i++) {
          ids.push(checkRows[i].id)
        }
        that.$store.dispatch('upDownSupportServices', {
          ids: ids.join(','),
          state: state,
          fn (data) {
            if (data.code === 0) {
              if (state === '1') {
                that.$message({
                  type: 'success',
                  message: '上架成功！'
                })
              }
              if (state === '0') {
                that.$message({
                  type: 'success',
                  message: '下架成功！'
                })
              }
              that.getSupportServicesFn()
            } else {
              that.$message.error(data.msg)
            }
          }
        })
      },
      operationFn: function (num) { // 操作事件 0 添加 1 编辑 2 删除 3 上架 4 下架
        let that = this
        let checkRows = that.checkRows
        if (num === 0) {
          this.$router.push({path: '/servicesInfo', query: {operationType: 'add'}})
          return
        }
        if (checkRows.length > 0) {
          if (num === 1) {
            if (checkRows.length === 1) {
              that.$router.push({path: '/servicesInfo', query: {operationType: 'compile', id: checkRows[0].id}})
            } else {
              that.$message({
                type: 'warning',
                message: '不能批量编辑！'
              })
            }
          }
          if (num === 2) {
            that.removeSupportServicesFn()
          }
          if (num === 3) {
            that.upDownSupportServicesFn('1')
          }
          if (num === 4) {
            that.upDownSupportServicesFn('0')
          }
        } else {
          that.$message({
            type: 'warning',
            message: '请选择一行后在执行此操作！'
          })
        }
      },
      SizeChangeFn: function (pageNum) { // 显示条数触发
        this.pageNum = pageNum
      },
      CurrentChangeFn: function (page) { // 页数触发
        this.page = page
      },
      selectionFn: function (rows) { // 多选触发
        this.checkRows = rows
      }
    },
    mounted: function () {
      let that = this
      that.getSupportServicesFn()
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style rel="stylesheet/less" lang="less">
  #servicesList {
    .breadcrumb {
      margin-bottom: 16px;
    }
    .servicesList-Cont {
      box-sizing: border-box;
      padding: 16px;
      background-color: #fff;
    }
    .pagination {
      margin-top: 16px;
      .Left {
        float: right;
      }
    }
    .SEO {
      margin-bottom: 16px;
      .Left {
        float: left;
      }
      .Right {
        float: right;
      }
      .el-form-item {
        margin-bottom: 0;
      }
    }
  }
</style>
