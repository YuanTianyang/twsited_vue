<template>
  <div>
    <div style="text-align: left">
      <div style="display: inline">
        <label>关键字</label>
        <el-input v-model="params" placeholder="请输入内容" style="width: 150px"></el-input>

        <el-button type="primary" icon="search">查询</el-button>

        <router-link :to="'page1'">
          <el-button type="primary"><i class="el-icon-plus"></i>新增</el-button>
        </router-link>
      </div>
    </div>
    <div>
      <el-table :data="companies" stripe style="width: 100%">
        <el-table-column prop="companyName" label="公司名称" width="200px" align="center">
        </el-table-column>

        <el-table-column prop="companyShortName" label="公司简称" width="180px" align="center">
        </el-table-column>

        <el-table-column prop="telephone" label="联系电话" width="180px" align="center">
        </el-table-column>

        <el-table-column prop="manager" label="负责人" width="180px" align="center">
        </el-table-column>

        <el-table-column prop="mobilePhone" label="手机号" width="180px" align="center">
        </el-table-column>
        <el-table-column prop="address" label="公司地址" width="180px" align="center">

        </el-table-column>
        <el-table-column prop="created" label="创建时间" width="180px" align="center">
        </el-table-column>

        <el-table-column label="操作" width="180px" align="center">
          <template slot-scope="companyList">
            <router-link :to="{ path: 'page1', query: { id: companyList.row.id }}">
              <el-button type="text" size="small">修改</el-button>
            </router-link>
            <el-button type="text" size="small" @click="deleteCompany(companyList.row.id)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <div class="block" style="float: right;margin-right: 50px">
      <el-pagination
        @current-change="changePage"
        :current-page.sync="page"
        :page-size="limit"
        layout="total, prev, pager, next"
        :total="total">
      </el-pagination>
    </div>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        companies: Array.of(),
        params: '',
        page: 1,
        limit: 5,
        total: 1
      }
    },
    mounted () {
      this.getCompanies()
    },
    methods: {
      deleteCompany (id) {
        let that = this
        that.$confirm('是否要删除当前公司?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          let params = {ids: id}
          that.$http.delete(
            '/v1/company', {params}
          ).then(response => {
            let result = response.data
            if (result.code > 0) {
              that.$message({
                type: 'success',
                message: result.message
              })
              that.getCompanies()
            } else {
              that.$message({
                type: 'error',
                message: result.message
              })
            }
          })
        }).catch(() => {
          that.$message({
            type: 'info',
            message: '已取消删除'
          })
        })
      },
      getCompanies () {
        let that = this
        that.$http.get(
          '/v1/company/companiesByAppKey')
          .then(response => {
            console.log(response.data)
            that.companies = response.data.data
          })
      }
    }
  }
</script>
