/***       用户列表        ***/
<template>
  <div id="userList">
    <div class="breadcrumb">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/home' }">{{language.home}}</el-breadcrumb-item>
        <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="userList-order">
      <div class="SEO clear">
        <div class="Left">
          <el-form :inline="true" :model="SEO" class="demo-form-inline">
            <el-form-item class="selectDateRange">
              <el-date-picker
                v-model="searchTime"
                size="small"
                type="daterange"
                @change="selectTimeFn"
                placeholder="选择时间">
              </el-date-picker>
            </el-form-item>
            <!--客户等级-->
            <el-form-item class="customerGrade" style="width:120px">
              <el-select v-model="serachVal.grade" filterable :placeholder="language.customerGrade"
                         clearable
                         size="small">
                <el-option
                  v-for="item in gradeList"
                  :key="item.id"
                  :label="item.gradeName"
                  :value="item.id">
                </el-option>
              </el-select>
            </el-form-item>
            <!--分配状态-->
            <el-form-item class="assigned" style="width:120px">
              <el-select v-model="serachVal.allocated" filterable :placeholder="language.assigned"
                         clearable
                         size="small">
                <el-option
                  v-for="item in allocates"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </el-form-item>
            <!--客户类别-->
            <el-form-item class="customerNature" style="width:120px">
              <el-select v-model="serachVal.customerNature" filterable :placeholder="language.clientCategory"
                         clearable
                         size="small">
                <el-option
                  v-for="item in customerNature"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </el-form-item>
            <!--客户状态-->
            <el-form-item class="customerStatus">
              <el-select v-model="serachVal.state" filterable :placeholder="language.customerStatus"
                         clearable
                         size="small">
                <el-option
                  v-for="item in customerState"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </el-form-item>
            <!--客户类别-->
            <el-form-item class="customerStatus">
              <el-input v-model="serachVal.elasticSearch" size="small" placeholder="请输入内容"></el-input>
            </el-form-item>
            <!--搜索-->
            <el-form-item class="">
              <el-button type="primary" size="small" @click="getCustomerssFn">{{language.search}}</el-button>
            </el-form-item>
          </el-form>
        </div>
        <div class="Right">
          <el-form :inline="true" class="demo-form-inline" style="width: 160px">
            <el-form-item class="selectDateRange">
              <el-dropdown split-button type="primary" @click="operationFn('14')" @command="operationFn" size="small"
                           trigger="click">
                查看详情
                <el-dropdown-menu slot="dropdown">
                  <el-dropdown-item command="0">分配售前客服</el-dropdown-item>
                  <el-dropdown-item command="10">分配售后客服</el-dropdown-item>
                  <el-dropdown-item command="11">资金管理</el-dropdown-item>
                  <el-dropdown-item command="12">跟踪记录</el-dropdown-item>
                  <el-dropdown-item command="5">会员等级</el-dropdown-item>
                  <el-dropdown-item command="1">发送短信</el-dropdown-item>
                  <el-dropdown-item command="2">发送邮件</el-dropdown-item>
                  <el-dropdown-item command="3">发送微信通知</el-dropdown-item>
                  <el-dropdown-item command="4">发送站内信</el-dropdown-item>
                  <el-dropdown-item command="6">导入老用户</el-dropdown-item>
                  <el-dropdown-item command="7">导出会员</el-dropdown-item>
                  <el-dropdown-item command="8">解冻</el-dropdown-item>
                  <el-dropdown-item command="9">冻结</el-dropdown-item>
                  <el-dropdown-item command="13">修改密码</el-dropdown-item>
                </el-dropdown-menu>
              </el-dropdown>
            </el-form-item>
          </el-form>
        </div>
      </div>
      <div class="order-table">
        <el-table
          v-loading="loading"
          ref="multipleTable"
          :data="customerData"
          border
          tooltip-effect="dark"
          style="width: 100%"
          @selection-change="selectionFn">
          <el-table-column
            fixed
            type="selection"
            width="55">
          </el-table-column>
          <el-table-column
            label="注册时间"
            prop="registerTime"
            min-width="160">
          </el-table-column>
          <el-table-column
            label="用户等级"
            prop="gradeName"
            min-width="120">
          </el-table-column>
          <el-table-column
            label="用户来源"
            prop="source"
            min-width="120">
          </el-table-column>
          <el-table-column
            label="姓名"
            prop="username"
            min-width="120">
          </el-table-column>
          <el-table-column
            label="性别"
            prop="sexual"
            min-width="120">
          </el-table-column>
          <el-table-column
            label="手机号"
            prop="customerPhone"
            min-width="140">
          </el-table-column>
          <el-table-column
            label="邮箱"
            prop="email"
            min-width="200">
          </el-table-column>
          <el-table-column
            label="售前客服"
            prop="servicePerson"
            min-width="100">
          </el-table-column>
          <el-table-column
            label="售后客服"
            prop="afterWorking"
            min-width="100">
          </el-table-column>
          <el-table-column
            label="所在地"
            prop="registerAddress"
            min-width="120">
          </el-table-column>
          <el-table-column
            label="最近登陆时间"
            prop="recentLoginTime"
            min-width="180">
          </el-table-column>
          <el-table-column
            label="状态"
            prop="state"
            min-width="80">
          </el-table-column>
        </el-table>
      </div>
      <div class="pagination clear">
        <div class="Right">
          <el-pagination
            @size-change="SizeChangeFn"
            @current-change="CurrentChangeFn"
            :current-page="page"
            :page-sizes="[20, 40, 60, 80]"
            :page-size="pageNum"
            layout="total, sizes, prev, pager, next, jumper"
            :total=parseInt(totals)>
          </el-pagination>
        </div>
      </div>
    </div>
    <div class="Box">
      <el-dialog :title="BoxTitle" :visible.sync="boxShow" @close="boxCloseFn" size="small">
        <el-form labelWidth="80px" v-if="boxType===0||boxType===5||boxType===10">
          <el-form-item label="用户等级" v-if="boxType===5">
            <el-select v-model="checkedGradeID" filterable placeholder="请选择等级" clearable>
              <el-option
                v-for="item in gradeList"
                :key="item.value"
                :label="item.gradeName"
                :value="item.id">
              </el-option>
            </el-select>
          </el-form-item>
          <!--分配售前客服-->
          <el-form-item v-if="boxType===0">
            <el-form>
              <el-form-item class="afterSaleService-search">
                <el-input v-model="serviceVal" auto-complete="off" placeholder="姓名/员工号"></el-input>
                <el-button type="primary" @click="getAllServiceManFn">搜索</el-button>
              </el-form-item>
              <el-form-item class="allSaleServices-table">
                <el-table :data="allSaleServices" @selection-change="selectionSaleServicesFn" ref="preServiceTable">
                  <el-table-column type="selection" width="55"></el-table-column>
                  <el-table-column property="account" label="工号" width="120"></el-table-column>
                  <el-table-column property="customerCount" label="手上现有客户" width="200"></el-table-column>
                  <el-table-column property="name" label="姓名"></el-table-column>
                  <el-table-column property="phone" label="电话"></el-table-column>
                </el-table>
              </el-form-item>
            </el-form>
            <div class="pagination">
              <el-pagination
                @size-change="serviceSizeChangFn"
                @current-change="serviceCurrentChangeFn"
                :current-page=prePage
                :page-sizes="[10, 20, 30, 40]"
                :page-size=preSize
                layout="total, sizes, prev, pager, next, jumper"
                :total=allSaleServicesTotal>
              </el-pagination>
            </div>
          </el-form-item>
          <!--分配售后客服-->
          <el-form-item v-if="boxType===10">
            <el-form>
              <el-form-item class="afterSaleService-search">
                <el-input v-model="serviceVal" auto-complete="off" placeholder="姓名/员工号"></el-input>
                <el-button type="primary" @click="getAllServiceManFn">搜索</el-button>
              </el-form-item>
              <el-form-item class="allSaleServices-table">
                <el-table :data="allSaleServices" @selection-change="selectionAfterSaleServicesFn"
                          ref="afterServiceTable">
                  <el-table-column type="selection" width="55"></el-table-column>
                  <el-table-column property="account" label="工号" width="120"></el-table-column>
                  <el-table-column property="customerCount" label="手上现有客户" width="200"></el-table-column>
                  <el-table-column property="name" label="姓名"></el-table-column>
                  <el-table-column property="phone" label="电话"></el-table-column>
                </el-table>
              </el-form-item>
            </el-form>
            <div class="pagination">
              <el-pagination
                @size-change="serviceAfterSizeChangFn"
                @current-change="serviceAfterCurrentChangeFn"
                :current-page=afterPage
                :page-sizes="[10, 20, 30, 40]"
                :page-size=afterSize
                layout="total, sizes, prev, pager, next, jumper"
                :total=allSaleServicesTotal>
              </el-pagination>
            </div>
          </el-form-item>
        </el-form>
        <!--发送通知4-->
        <!--发送短信2-->
        <!--发送微信3-->
        <el-form labelWidth="100px" v-if="boxType===4||boxType===2||boxType===1||boxType===3">
          <el-form-item label="选择模板：">
            <el-select v-model="templateClassify" @change="setTemplateData">
              <template v-for="(item,index) in templateData">
                <el-option :label=item.templateName :value=index></el-option>
              </template>
            </el-select>
          </el-form-item>
          <el-form-item label="标题：">
            <el-input v-model="sendMessageFrom.title"></el-input>
          </el-form-item>
          <el-form-item label="发送地址：" v-if="boxType===2">
            {{`本次发送共计` + checkUser.checkUserEmail.length + `个地址`}}
          </el-form-item>
          <el-form-item label="内容：">
            <el-input type="textarea" v-model="sendMessageFrom.content" v-if="boxType===1||boxType===3"></el-input>
            <vue-editor v-model="sendMessageFrom.content" v-if="boxType===2||boxType===4"></vue-editor>
          </el-form-item>
        </el-form>
        <p v-if="boxType === 8">您确定要解冻该用户吗？</p>
        <p v-if="boxType === 9">您确定要冻结该用户吗？</p>
        <!--确认-->
        <div slot="footer" class="dialog-footer">
          <el-button @click="boxOperationFn(0)">取 消</el-button>
          <el-button type="primary" @click="boxOperationFn(1),boxShow=false" :loading="boxLoading">确 定</el-button>
        </div>
      </el-dialog>
      <!--跟踪记录-->
      <el-dialog title="跟踪记录" :visible.sync="traceState">
        <el-form :model="traceRecord" :rules="rules">
          <el-form-item label="意向金额" :label-width="formLabelWidth" prop="money">
            <el-input
              size="small"
              placeholder="请输入意向金"
              v-model="traceRecord.money" auto-complete="off" type="number"></el-input>
          </el-form-item>
          <el-form-item label="跟进情况" :label-width="formLabelWidth" prop="content">
            <el-input type="textarea"
                      :rows="3"
                      placeholder="请输入内容"
                      resize="none"
                      v-model="traceRecord.content" auto-complete="off"></el-input>
          </el-form-item>
          <el-form-item :label-width="formLabelWidth" prop="traceSelect">
            <el-radio-group v-model="traceRecord.traceSelect">
              <el-radio
                size="small"
                :label="1">普通
              </el-radio>
              <el-radio
                size="small"
                :label="2">重点
              </el-radio>
              <el-radio
                size="small"
                :label="3">排签
              </el-radio>
              <el-radio
                size="small"
                :label="4">公海
              </el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item class="flex-end">
            <el-button @click="traceState = false" size="small">取 消</el-button>
            <el-button type="primary" @click="postTrackRecordFn(traceRecord)" size="small">添加</el-button>
          </el-form-item>
        </el-form>
        <div>
          <h5 style="margin-bottom: 16px">跟进记录</h5>
          <el-table
            size="small"
            :data="traceData"
            border
            style="width: 100%">
            <el-table-column
              type="index"
              label="序号"
              width="80">
            </el-table-column>
            <el-table-column
              prop="typesName"
              label="类型">
            </el-table-column>
            <el-table-column
              prop="createTime"
              label="日期"
              width="180">
            </el-table-column>
            <el-table-column
              prop="interestAmount"
              label="意向金(元)"
              width="180">
            </el-table-column>
            <el-table-column
              prop="adminName"
              label="处理人"
              width="90">
            </el-table-column>
            <el-table-column
              prop="recentSituation"
              label="反馈情况">
            </el-table-column>
          </el-table>
        </div>
      </el-dialog>
      <!--修改密码-->
      <el-dialog title="修改密码" :visible.sync="changePasswordState">
        <el-form :model="updatePassword" :rules="rules">
          <el-form-item label="新密码" :label-width="formLabelWidth" prop="password">
            <el-input
              size="small"
              placeholder="请输入新密码"
              v-model="updatePassword.password" auto-complete="off" type="password"></el-input>
          </el-form-item>
          <el-form-item label="再输一遍" :label-width="formLabelWidth" prop="passwordAgain">
            <el-input
              size="small"
              placeholder="请再输入一边新密码"
              v-model="updatePassword.passwordAgain" auto-complete="off" type="password"></el-input>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="changePasswordState = false" size="small">取 消</el-button>
          <el-button type="primary" @click="putUpdatePasswordFn(updatePassword)" size="small">确 定</el-button>
        </div>
      </el-dialog>
      <!--导入-->
      <!--<el-upload-->
        <!--class="upload-demo"-->
        <!--action="v1/guide "-->
        <!--:on-preview="handlePreview"-->
        <!--:on-remove="handleRemove"-->
        <!--multiple-->
        <!--:limit="3"-->
        <!--:on-exceed="handleExceed"-->
        <!--:file-list="fileList">-->
        <!--<el-button size="small" type="primary" class="upload-button">点击上传</el-button>-->
      <!--</el-upload>-->
    </div>
  </div>
</template>

<script>
  import cookie from '../../mixin/cookie'
  import common from '../../mixin/common'
  import fileOperation from '../../mixin/ImportAndexport'

  export default {
    components: {},
    name: 'userList',
    computed: {
      languageType: function () {
        return this.$store.getters.languageType
      },
      getCustomerss: function () {
        return this.$store.getters.getCustomerss
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
        traceRecord: {
          money: '',
          content: '',
          traceSelect: 1,
          id: '',
          customerId: ''
        },
        rules: {
          password: [
            {required: true, message: '请输入新密码', trigger: 'blur'}
          ],
          passwordAgain: [
            {required: true, message: '请再输入新密码', trigger: 'blur'}
          ],
          money: [
            {required: true, message: '请输入意向金', trigger: 'blur'}
          ],
          content: [
            {required: true, message: '请再输入跟进情况', trigger: 'blur'}
          ],
          traceSelect: [
            {required: true, message: '请选择类别', trigger: 'blur'}
          ]
        },
//        修改密码
        changePasswordState: false,
        traceSelect: 0,
        formLabelWidth: '120px',
//        跟踪记录状态
        traceState: false,
//        跟踪记录数据
        traceData: [],
        searchTime: '',
        SEO: { // 搜索
          timeHorizon: '', // 时间范围
          userGrade: '', // 用户等级
          clientCategory: '' // 用户等级
        },
        loading: false, // 加载
        checkedRows: [], // 选中的 rows
        options: [],
        boxShow: false, // 弹出框状态
        boxLoading: false, // 弹出框加载
        BoxTitle: '', // 弹出框标题
        boxType: '', // 弹出框操作类型
        gradeList: [], // 等级列表
        checkedGrade: '', // 选中等级
        checkedGradeID: '', // 等选中等级ID
        page: 1, // 页数
        pageNum: 20, // 每页条数
        CustomersList: [], // 客户列表
        clientCategory: [], // 客户类别
        assigned: [], // 分配状态
        customerGrade: [], // 客户等级
        customerStatus: [], // 客户状态
        getCustomerssTemp: [],
        serachVal: {
          start: '',
          end: '',
          grade: '',
          allocated: '',
          state: '',
          elasticSearch: '',
          customerNature: ''
        },
        offset: 1,
        size: 20,
//        分配类别
        allocates: [
          {
            value: 0,
            label: '未分配'
          }, {
            value: 1,
            label: '已分配'
          }
        ],
//        客户性质
        customerNature: [
          {
            value: 1,
            label: '新客户'
          }, {
            value: 2,
            label: '已开试用'
          }, {
            value: 3,
            label: '已正式购买'
          }, {
            value: 4,
            label: '老系统'
          }
        ],
//        客户状态
        customerState: [
          {
            value: 0,
            label: '冻结'
          }, {
            value: 1,
            label: '正常'
          }
        ],
//        客户数据
        customerData: [],
        totals: '',
        serviceVal: '',
//        所有客服
        allSaleServices: [],
//        选中的售前客服
        preSaleService: [],
        prePage: 1,
        preSize: 20,
//        选中的售后客服
        afterSaleService: [],
        afterPage: 1,
        afterSize: 10,
        allSaleServicesTotal: '',
//        发送消息
        sendMsgForm: {
          title: '',
          content: ''
        },
//        选中用户
        checkUser: {
          checkUserEmail: [],
          checkUserIds: [],
          checkUserCustomerPhones: [],
          checkUserOpenids: []
        },
//        模板类型
        templateClassify: '',
        sendMessageFrom: {
          title: '',
          content: ''
        },
        templateData: [],
//        修改密码
        updatePassword: {
          id: '',
          password: '',
          passwordAgain: ''
        },
        customerId: []
      }
    },
    mixins: [cookie, common, fileOperation],
    watch: {
      boxShow: function (val) {
        if (!val) {
          this.templateClassify = ''
          this.sendMessageFrom.title = ''
          this.sendMessageFrom.content = ''
        }
      }
    },
    methods: {
      handleRemove (file, fileList) {
        console.log(file, fileList)
      },
      handlePreview (file) {
        console.log(file)
      },
      handleExceed (files, fileList) {
        this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`)
      },
      getUserGradesFn: function () { // 获取用户等级
        let that = this
        that.$store.dispatch('getUserGrades', {
          fn (data) {
            if (data.code === 0) {
              that.gradeList = data.data
            }
          }
        })
      },
      getCustomerssFn: function () { // 获取用户列表
        let that = this
        that.loading = true
        that.$store.dispatch('getCustomerss', {
          start: that.serachVal.start, // 开始时间
          end: that.serachVal.end, // 开始时间
          grade: that.serachVal.grade, // 等级
          allocated: that.serachVal.allocated, // 分配状态
          state: that.serachVal.state, // 状态
          elasticSearch: that.serachVal.elasticSearch, // 模糊查询
          customerNature: that.serachVal.customerNature, // 客户类别
          offset: that.page, // 页数
          size: that.pageNum,  // 页面长度
          fn (data) {
            that.loading = false
            if (data && data.code === 0) {
              data.data.content.forEach(function (value) {
                value.state = value.state === 0 ? '已冻结' : '正常'
                value.recentLoginTime = that.$Tool.TimestampDate(value.recentLoginTime)
                value.registerTime = that.$Tool.TimestampDate(value.registerTime * 1000)
                value.sexual = value.sexual === 0 ? '女' : '男'
                value.source = value.source === 1 ? '网站注册' : '老系统导入'
              })
              that.customerData = data.data.content
              that.totals = data.data.totals
            }
          }
        })
      },
      selectionFn: function (row) { // table  多选触发
        this.checkedRows = row
        this.formatCheckedRowsFn(row)
      },
      putCustomerFn: function (num) { // 0 分配售前  10 分配售后 5 会员等级 4发送站内信 8 解冻 9 冻结
        let that = this
        if (num === 7) {
          that.getExportGuideFn()
        } else {
          //        选中的用户
          let checkedRows = that.checkedRows
          let ids = []
          num = Number(num)
//        选中用户的ids
          for (let i = 0; i < checkedRows.length; i++) {
            ids.push(checkedRows[i].id)
          }
          if (num === 0 || num === 5 || num === 8 || num === 9 || num === 10) {
            let params = {
//          数组to字符串
              options: ids.join(','), // 选中客户ID
              state: '', // 客户状态
              fn (data) {
                that.boxLoading = false
                if (data && data.code === 0) {
                  let message = ''
                  switch (num) {
                    case 0:
                      message = '分配售前客服成功！'
                      break
                    case 5:
                      message = '设置等级成功！'
                      break
                    case 8:
                      message = '解冻成功！'
                      break
                    case 9:
                      message = '冻结成功！'
                      break
                    case 10:
                      message = '分配售后客服成功！'
                      break
                  }
                  that.$message({
                    type: 'success',
                    message: message
                  })
                  that.getCustomerssFn()
                  that.boxShow = false
                } else {
                  that.$message({
                    message: data.msg,
                    type: 'warning'
                  })
                }
              }
            }
            if (num === 0) {
              if (that.preSaleService.length !== 1) {
                that.$message({
                  message: '请选择一个客服进行操作',
                  type: 'warning'
                })
              } else {
                that.preSaleService = that.preSaleService[0].id
                params['servicePersonId'] = that.preSaleService // 售前
              }
            }
            if (num === 10) {
              if (that.afterSaleService.length !== 1) {
                that.$message({
                  message: '请选择一个客服进行操作',
                  type: 'warning'
                })
              } else {
                that.afterSaleService = that.afterSaleService[0].id
                params['afterSalesPersonId'] = that.afterSaleService // 售后
              }
            }
            if (num === 5) {
              params['gradeId'] = that.checkedGradeID // 设置等级ID
            }
//            冻结
            if (num === 9) {
              let state = true
              for (let i = 0; i < checkedRows.length; i++) {
                if (checkedRows[i].state === '已冻结') {
                  state = false
                }
              }
              if (state) {
                params['state'] = 0
              } else {
                that.$message({
                  message: '已冻结用户不能执行该操作',
                  type: 'warning'
                })
              }
            }
//            解冻
            if (num === 8) {
              let state = true
              for (let i = 0; i < checkedRows.length; i++) {
                if (checkedRows[i].state === '正常') {
                  state = false
                }
              }
              if (state) {
                params['state'] = 1
              } else {
                that.$message({
                  message: '正常用户不能执行该操作',
                  type: 'warning'
                })
              }
            }
            that.boxLoading = true
            that.$store.dispatch('putCustomer', params)
          }
          if (num === 2 || num === 3 || num === 4 || num === 1) {
            switch (num) {
//            发送短信
              case 1:
                that.postSendSmsFn()
                break
//            发送邮件
              case 2:
                that.postSendEmail()
                break
//            发送站内信
              case 4:
                that.postSendInformFn()
                break
//            发送微信
              case 3:
                that.postSendWechatFn()
                break
            }
          }
        }
      },
      boxOperationFn: function (num) { // Box操作 0 取消 1 确定
        let that = this
        if (num === 0) {
          that.boxShow = false
          return
        }
        if (num === 1) {
          that.putCustomerFn(that.boxType)
        }
      },
      operationFn: function (num) { // 操作菜单触发 0 分配客户 5 会员等级 1 发送短信 2 发送邮件 4 发送站内信 3 发送微信 6 导入老客户 7 导出会员 8 解冻 9 冻结
        let that = this
        num = Number(num)
        if (num === 7) {
          that.getExportGuideFn()
        } else if (num === 6) {
          document.querySelector('.upload-button')
        } else {
          let checkedRows = that.checkedRows
          if (checkedRows.length > 0) {
            switch (num) {
              case 0:
                that.BoxTitle = '分配售前客服'
                that.getAllServiceManFn()
                that.boxShow = true
                break
              case 5:
                that.BoxTitle = '设置用户等级'
                that.boxShow = true
                that.getUserGradesFn()
                break
              case 1:
                that.BoxTitle = '发送短信'
                that.boxShow = true
                that.boxType = 1
                that.searchBtnFn(1)
                break
              case 2:
                that.BoxTitle = '发送邮件'
                that.boxShow = true
                that.boxType = 2
                that.searchBtnFn(2)
                break
              case 4:
                that.BoxTitle = '发送站内信'
                that.boxShow = true
                that.boxType = 4
                that.searchBtnFn(4)
                break
              case 3:
                that.BoxTitle = '发送微信'
                that.boxShow = true
                that.boxType = 3
                that.searchBtnFn(3)
                break
              case 8:
                that.BoxTitle = '解冻'
                that.boxShow = true
                break
              case 9:
                that.BoxTitle = '冻结'
                that.boxShow = true
                break
              case 10:
                that.serviceVal = ''
                that.BoxTitle = '分配售后客服'
                that.boxShow = true
                that.getAllServiceManFn()
                break
            }
            if (num === 0 || num === 5 || num === 8 || num === 9 || num === 10) {
              that.boxType = num
            }
            if (num === 11 || num === 12 || num === 13 || num === 14) {
              if (checkedRows.length === 1) {
                switch (num) {
                  case 11:
                    that.$router.push({path: '/moneyAccounts', query: {id: checkedRows[0].id}})
                    break
                  case 12:
                    that.traceRecord.id = checkedRows[0].id
                    that.traceRecord.customerId = checkedRows[0].customerId
                    that.getTrackRecordFn(checkedRows[0])
                    that.traceState = true
                    break
                  case 13:
                    that.updatePassword.id = checkedRows[0].id
                    that.changePasswordState = true
                    break
                  case 14:
                    that.$router.push({path: '/userDetail', query: {id: checkedRows[0].id}})
                    break
                }
              } else if (checkedRows.length > 1) {
                that.$message({
                  type: 'warning',
                  message: '请选择一个用户后再进行此操作！'
                })
              }
            }
          } else if (checkedRows.length === 0) {
            that.$message({
              type: 'warning',
              message: '请选择一行后再进行此操作！'
            })
          }
        }
      },
      SizeChangeFn: function (pageNum) { // 每页显示数量
        this.pageNum = pageNum
        this.getCustomerssFn()
      },
      CurrentChangeFn: function (page) { // 页数
        this.page = page
        this.getCustomerssFn()
      },
      boxCloseFn: function () { // box 关闭回调
        this.boxLoading = false
      },
//      选择时间范围
      selectTimeFn: function (time) { // 选择时间触发
        this.serachVal.start = new Date(time[0]).valueOf() / 1000
        this.serachVal.end = new Date(time[1]).valueOf() / 1000
      },
//      查询所有客服
      getAllServiceManFn: function () {
        let that = this
        that.$store.dispatch('getAllServiceMan', {
          page: that.afterPage,
          size: that.afterSize,
          name: that.serviceVal,
          fn (data) {
            if (data && data.code === 0) {
              that.allSaleServices = data.data
              that.allSaleServicesTotal = data.count
            } else {
              that.$message({
                message: '查询客服失败！',
                type: 'warning'
              })
            }
          }
        })
      },
//      更改条数
      serviceSizeChangFn: function (size) {
        this.afterSize = size
        this.getAllServiceManFn()
      },
//      更改页数
      serviceCurrentChangeFn: function (offset) {
        this.afterPage = offset
        this.getAllServiceManFn()
      },
      //      更改条数
      serviceAfterSizeChangFn: function (size) {
        this.afterSize = size
        this.getAllServiceManFn()
      },
//      更改页数
      serviceAfterCurrentChangeFn: function (offset) {
        this.afterPage = offset
        this.getAllServiceManFn()
      },
//       选中售前客服
      selectionSaleServicesFn: function (row) {
        this.preSaleService = row
      },
//      选中售后
      selectionAfterSaleServicesFn: function (row) {
        this.afterSaleService = row
      },
//      发送站内信
      postSendInformFn: function () {
        let that = this
        that.$store.dispatch('postSendInform', {
          ids: that.customerId.toString(),
          tital: that.sendMessageFrom.title,
          content: that.sendMessageFrom.content,
          fn (data) {
            if (data && data.code === 0) {
              that.$message({
                message: '发送成功',
                type: 'success'
              })
            } else {
              that.$message({
                message: data.msg,
                type: 'warning'
              })
            }
          }
        })
      },
//       发送短信
      postSendSmsFn: function () {
        let that = this
        that.$store.dispatch('postSendSms', {
          ids: that.customerId.toString(),
          tital: that.sendMessageFrom.title,
          content: that.sendMessageFrom.content,
          fn (data) {
            if (data && data.code === 0) {
              that.$message({
                message: '发送成功',
                type: 'success'
              })
            } else {
              that.$message({
                message: data.msg,
                type: 'warning'
              })
            }
          }
        })
      },
//      发送微信
      postSendWechatFn: function () {
        let that = this
        if (that.customerId === null) {
          that.$message({
            message: '用户没有绑定微信',
            type: 'warning'
          })
        } else {
          that.$store.dispatch('postSendWechat', {
            ids: that.customerId.toString(),
            tital: that.sendMessageFrom.title,
            content: that.sendMessageFrom.content,
            fn (data) {
              if (data && data.code === 0) {
                that.$message({
                  message: '发送成功',
                  type: 'success'
                })
              } else {
                that.$message({
                  message: data.msg,
                  type: 'warning'
                })
              }
            }
          })
        }
      },
//      发送邮件
      postSendEmail: function () {
        let that = this
        that.$store.dispatch('postSendEmail', {
          ids: that.customerId.toString(),
          tital: that.sendMessageFrom.title,
          content: that.sendMessageFrom.content,
          fn (data) {
            if (data && data.code === 0) {
              that.$message({
                message: '发送成功',
                type: 'success'
              })
            } else {
              that.$message({
                message: data.msg,
                type: 'warning'
              })
            }
          }
        })
      },
      formatCheckedRowsFn: function (obj) {
        let arr = []
        for (let i in obj) {
          arr.push(obj[i])
        }
        let that = this
        that.customerId = []
        arr.forEach(function (value) {
          that.customerId.push(value.id)
        })
      },
//      搜索模板
      searchBtnFn: function (type) {
        let that = this
        that.$store.dispatch('getCustomTemplateByType', {
          templateType: type,
          offset: 1,
          size: 20,
          fn (data) {
            if (data.code === 0) {
              data.data.content.forEach(function (value) {
                switch (parseInt(value.templateType)) {
                  case 1:
                    value.templateType = '短信模板'
                    break
                  case 2:
                    value.templateType = '邮件模板'
                    break
                  case 3:
                    value.templateType = '微信模板'
                    break
                  case 4:
                    value.templateType = '站内信模板'
                    break
                }
              })
              that.templateData = data.data.content
            }
          }
        })
      },
//      设置发送数据
      setTemplateData: function () {
//        templateClassify 选中的id  sendMessageFrom双向绑定 templateData
        this.sendMessageFrom.title = this.templateData[this.templateClassify].title
        this.sendMessageFrom.content = this.templateData[this.templateClassify].content
      },
//      导出
      getExportGuideFn: function () {
        let that = this
        that.$store.dispatch('getExportGuide', {
          tokens: that.tokens.tokens,
          start: that.serachVal.start, // 开始时间
          end: that.serachVal.end, // 开始时间
          grade: that.serachVal.grade, // 等级
          allocated: that.serachVal.allocated, // 分配状态
          state: that.serachVal.state, // 状态
          elasticSearch: that.serachVal.elasticSearch, // 模糊查询
          customerNature: that.serachVal.customerNature, // 客户类别
          fn (data) {
            if (data) {
            }
          }
        })
      },
//      修改密码
      putUpdatePasswordFn: function (obj) {
        let that = this
        if (obj.password === obj.passwordAgain) {
          that.$store.dispatch('putUpdatePassword', {
            customerId: obj.id,
            password: obj.password,
            fn (data) {
              if (data && data.code === 0) {
                that.$message({
                  message: '修改成功',
                  type: 'success'
                })
                that.changePasswordState = false
              } else {
                that.$message({
                  message: '修改失败',
                  type: 'warning'
                })
              }
              that.emptyObject(obj)
            }
          })
        } else {
          that.$message({
            message: '两次密码输入不正确',
            type: 'warning'
          })
          that.emptyObject(obj)
        }
      },
      //       添加纪录
      postTrackRecordFn: function (obj) {
        let that = this
        that.$store.dispatch('postTrackRecord', {
          customerId: obj.id,
          interestAmount: obj.money,
          recentSituation: obj.content,
          types: obj.traceSelect,
          fn (data) {
            if (data && data.code === 0) {
              that.$message({
                message: '添加成功',
                type: 'success'
              })
              that.emptyObject(obj)
              that.getTrackRecordFn(that.checkedRows[0])
            } else {
              that.$message({
                message: '添加失败',
                type: 'warning'
              })
            }
          }
        })
      },
      //       查询纪录
      getTrackRecordFn: function (obj) {
        let that = this
        that.$store.dispatch('getTrackRecord', {
          customerId: obj.id,
          fn (data) {
            if (data && data.code === 0) {
              data.data.forEach(function (val) {
                val.createTime = that.$Tool.TimestampDate(val.createTime)
                switch (val.types) {
                  case 1:
                    val.typesName = '普通'
                    break
                  case 2:
                    val.typesName = '重点'
                    break
                  case 3:
                    val.typesName = '排签'
                    break
                  case 4:
                    val.typesName = '公海'
                    break
                }
              })
              that.traceData = data.data
            } else {
              that.$message({
                message: '查询失败',
                type: 'warning'
              })
            }
          }
        })
      },
//      导出模板
      getExportCustomerTemplateFn: function () {
        let that = this
        that.$store.dispatch('getExportCustomerTemplate', {
          tokens: that.tokens.tokens,
          fn (data) {
            if (data) {
              var blob = new Blob([data], {type: 'application/ms-excel'})
              var fileName = 'details.xls'
              that.downloadFileMixin(blob, fileName)
            }
          }
        })
      }
    },
    created: function () {
    },
    mounted: function () {
      let that = this
      that.getCustomerssFn()
      that.getUserGradesFn()
//      that.getExportCustomerTemplateFn()
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style rel="stylesheet/less" lang="less">
  #userList {
    .breadcrumb {
      margin-bottom: 16px;
    }
    .userList-order {
      margin-top: 16px;
      box-sizing: border-box;
      padding: 16px;
      background-color: #fff;
      overflow: hidden;
    }
    .pagination {
      margin-top: 16px;
      .Right {
        float: right;
      }
    }
    .SEO {
      margin-bottom: 16px;
      .customerStatus, .clientCategory, .assigned, .customerGrade {
        width: 100px;
      }
      .selectDateRange {
        .el-date-editor--daterange.el-input {
          width: 200px;
        }
      }
      .el-form-item {
        margin-bottom: 0;
      }
      .Left {
        width: calc(~'100% - 120px');
        float: left;
      }
      .Right {
        width: 110px;
        float: right;
      }
    }
    .afterSaleService-search > .el-form-item__content {
      display: flex;
      .el-input {
        width: 240px;
        margin-right: 16px;
      }
    }
    .allSaleServices-table {
      margin-top: 16px;
    }
    .flex-end {
      display: flex;
      justify-content: flex-end;
    }
  }
</style>
