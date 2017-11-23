<template>
  <div>
    <div style="text-align: left">
      <div style="display: inline">
        <label>关键字</label>
        <el-input v-model="params" placeholder="请输入内容" style="width: 150px"></el-input>

        <label>注册时间</label>
        <el-date-picker v-model="createdRange" type="daterange" :editable=false></el-date-picker>

        <label>帐号状态</label>
        <el-select v-model="userStatu" placeholder="请选择">
          <el-option
            v-for="item in userStatus"
            :key="item.statuKey"
            :label="item.statuValue"
            :value="item.statuKey">
          </el-option>
        </el-select>

        <el-button type="primary" icon="search">查询</el-button>

      </div>
    </div>
    <div style="text-align: left">
      <div style="display: inline">
        <router-link :to="'page2'">
          <el-button type="primary"><i class="el-icon-plus"></i>新增</el-button>
        </router-link>
      </div>

      <div style="display: inline">
        <el-button type="primary" @click="joinOrRemoveBlackList(ids,1)"><i class="el-icon-document"></i>黑名单</el-button>
      </div>

      <div style="display: inline">
        <el-button type="primary" @click="deletePassenger(ids)"><i class="el-icon-delete"></i>删除</el-button>
      </div>
    </div>
    <div>
      <el-table :data="passengers" stripe highlight-current-row style="width: 100%" @selection-change="selectedRows">

        <el-table-column type="selection" width="55">
        </el-table-column>

        <el-table-column prop="name" label="用户名称" width="200px" align="center">
        </el-table-column>

        <el-table-column prop="phone" label="手机号" width="180px" align="center">
        </el-table-column>

        <el-table-column prop="companyName" label="所属公司" width="180px" align="center">
        </el-table-column>

        <el-table-column prop="gender" label="性别" width="150px" align="center">
          <template slot-scope="passengerList">
            <p v-if="passengerList.row.gender === 1">男</p>
            <p v-if="passengerList.row.gender === 2">女</p>
          </template>
        </el-table-column>

        <el-table-column prop="birthday" label="生日" width="180px" align="center">
        </el-table-column>

        <el-table-column prop="created" label="注册日期" width="180px" align="center">
        </el-table-column>

        <el-table-column prop="created" label="消费金额" width="180px" align="center">
        </el-table-column>

        <el-table-column prop="created" label="最后登录时间" width="180px" align="center">
        </el-table-column>

        <el-table-column prop="blacklist" label="帐号状态" width="80px" align="center">
          <template slot-scope="passengerList">
            <p v-if="passengerList.row.blacklist === 1">正常</p>
            <p v-if="passengerList.row.blacklist === 2">禁用</p>
          </template>
        </el-table-column>

        <el-table-column label="操作" width="300px" align="center">
          <template slot-scope="passengerList">
            <router-link :to="{ path: 'page2', query: { id: passengerList.row.id }}">
              <el-button type="warning" size="small">修改</el-button>
            </router-link>
            <el-button size="small" @click="joinOrRemoveBlackList(passengerList.row.id,passengerList.row.blacklist)">黑名单
            </el-button>
            <el-button type="danger" size="small" @click="deletePassenger(passengerList.row.id)">删除</el-button>
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
        passengers: Array.of(),
        ids: '',
        page: 1,
        limit: 5,
        total: 1,
        params: '',
        createdRange: '',
        userStatu: '',
        userStatus: [{
          statuKey: '1',
          statuValue: '正常'
        }, {
          statuKey: '2',
          statuValue: '禁用'
        }]
      }
    },
    mounted () {
      this.getPassengers(this.page, this.limit)
    },
    methods: {
      selectedRows (rows) {
        let that = this
        that.ids = ''
        if (rows.length > 0) {
          rows.forEach((row, index) => {
            let id = row.id
            if (index !== (rows.length - 1)) {
              that.ids += id + ','
            } else {
              that.ids += id
            }
          })
        } else {
          return false
        }
      },
      joinOrRemoveBlackList (id, blacklist) {
        let that = this
        let notice = blacklist === 1 ? '是否要将当前用户加入黑名单' : '是否要解除当前用户黑名单状态'
        let blackListUrl = blacklist === 1 ? '/v1/user/joinBlacklist' : '/v1/user/removeBlacklist'
        that.$confirm(notice, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          that.$http.post(
            blackListUrl, {
              ids: id
            },
            {
              emulateJSON: true
            }
          ).then(response => {
            let result = response.data
            if (result.code > 0) {
              that.$message({
                type: 'success',
                message: result.message
              })
              that.getPassengers()
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
            message: '已取消黑名单操作'
          })
        })
      },
      deletePassenger (id) {
        let that = this
        that.$confirm('是否要删除当前用户?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          let params = {ids: id}
          that.$http.delete(
            '/v1/user', {params}
          ).then(response => {
            let result = response.data
            if (result.code > 0) {
              that.$message({
                type: 'success',
                message: result.message
              })
              that.getPassengers()
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
      changePage (currentPage) {
        let that = this
        that.page = currentPage
        that.getPassengers()
      },
      getPassengers () {
        let that = this
        let params = {
          page: that.page,
          limit: that.limit
        }
        console.log(params)
        that.$http.get(
          '/v1/user',
          {params})
          .then(response => {
            console.log(response.data)
            that.passengers = response.data.data.passengerInfoList
            that.total = response.data.data.total
          })
      }
    }
  }
</script>
