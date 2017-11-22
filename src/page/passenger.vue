<template>
  <el-table :data="passengers" stripe style="width: 100%">
    <el-table-column prop="name" label="用户名称" width="200px" align="center">
    </el-table-column>

    <el-table-column prop="phone" label="手机号" width="180px" align="center">
    </el-table-column>

    <el-table-column prop="companyName" label="所属公司" width="180px" align="center">
    </el-table-column>

    <el-table-column prop="gender" label="性别" width="180px" align="center">
    </el-table-column>

    <el-table-column prop="birthday" label="生日" width="180px" align="center">
    </el-table-column>

    <el-table-column prop="created" label="注册时间" width="180px" align="center">
    </el-table-column>

    <el-table-column prop="created" label="消费金额" width="180px" align="center">
    </el-table-column>

    <el-table-column prop="created" label="最后登录时间" width="180px" align="center">
    </el-table-column>

    <el-table-column prop="blacklist" label="帐号状态" width="80px" align="center">
    </el-table-column>

    <el-table-column label="操作" width="300px" align="center">
      <template slot-scope="passengerList">
        <router-link :to="{ path: 'page2', query: { id: passengerList.row.id }}">
          <el-button type="warning" size="small">修改</el-button>
        </router-link>
        <el-button size="small">黑名单</el-button>
        <el-button type="danger" size="small" @click="deletePassenger(passengerList.row.id)">删除</el-button>
        <el-button type="info" size="small">密码重置</el-button>
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
  export default {
    data () {
      return {
        passengers: Array.of()
      }
    },
    mounted () {
      this.getPassengers()
    },
    methods: {
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
      getPassengers () {
        let that = this
        let params = {page: 1, limit: 20}
        that.$http.get(
          '/v1/user',
          {params})
          .then(response => {
            console.log(response.data)
            that.passengers = response.data.data
          })
      }
    }
  }
</script>
