<template>
  <div>
    <div>
      <el-form >
        <el-form-item label-width="300px" label="用户头像">
          <el-upload
          class="avatar-uploader"
          action="https://jsonplaceholder.typicode.com/posts/"
          :show-file-list="false"
          :on-success="handleAvatarSuccess"
          :before-upload="beforeAvatarUpload">
          <img v-if="userPhoto" :src="userPhoto" class="avatar">
          <i v-else class="el-icon-plus avatar-uploader-icon"></i>
          </el-upload>
        </el-form-item>
        <el-form-item label-width="300px" label="用户名称">
          <el-input v-model="userName" placeholder="请输入用户昵称"></el-input>
        </el-form-item>
        <el-form-item label-width="300px" label="手机号">
          <el-input v-model="userPhone" placeholder="请输入手机号"></el-input>
        </el-form-item>
        <el-form-item label-width="300px" label="所属公司">
          <el-select v-model="companyId" placeholder="请选择">
            <el-option
              v-for="company in companys"
              :key="company.companyId"
              :label="company.companyName"
              :value="company.companyId">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label-width="300px" label="性别">
          <el-radio class="radio" v-model="userGender" label="1">男</el-radio>
          <el-radio class="radio" v-model="userGender" label="2">女</el-radio>
        </el-form-item>
        <el-form-item label-width="300px" label="生日">
          <el-date-picker
            v-model="userBirthday"
            type="date"
            placeholder="选择日期"
            :picker-options="pickerOptions0">
          </el-date-picker>
        </el-form-item>
        <el-form-item label-width="300px" label="身份证号">
          <el-input v-model="userIdentity" placeholder="请输入身份证号"></el-input>
        </el-form-item>
        <el-form-item label-width="300px" label="帐号状态">
          <el-switch
            v-model="blackListState"
            :width="73"
            on-text="正常"
            off-text="已禁用"
            on-color="#13ce66"
            off-color="#ff4949"
            on-value="1"
            off-value="2">
          </el-switch>
        </el-form-item>
        <el-form-item  label-width="300px">
          <el-button type="info" @click="createUser()">确认</el-button>
          <el-button :plain="true" @click="getCompanies()">取消</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data () {
      return {
        pickerOptions0: {
          disabledDate (time) {
            return time.getTime() > Date.now() - 8.64e7
          }
        },
        value: '',
        userPhoto: '',
        userName: '',
        userPhone: '',
        userGender: '1',
        userBirthday: '',
        userIdentity: '',
        blackListState: '1',
        companys: [{
          companyId: 1,
          companyName: '滴滴科技'
        }, {
          companyId: 2,
          companyName: '易到用车'
        }, {
          companyId: 3,
          companyName: '小咖出行'
        }, {
          companyId: 4,
          companyName: '哒哒速运'
        }, {
          companyId: 5,
          companyName: 'ofo'
        }],
        companyId: ''
      }
    },
    computed: {
    },
    methods: {
      createUser () {
        let that = this
        let ssss
        that.companys.some(function (value, index, array) {
          if (array[index].companyId === that.companyId) {
            ssss = array[index].companyName
            return true
          }
        })
//        let allCompany = that.companys
//        let myCompanyName
//
//        for (let i = 0; i < allCompany.length; i++) {
//          alert(allCompany[i].companyId)
//          if (allCompany[i].companyId === that.companyId) {
//            myCompanyName = allCompany[i].companyName
//            break
//          }
//        }

        that.$http.post(
          '/v1/user',
          {
            'name': that.userName,
            'phone': that.userPhone,
            'companyId': that.companyId,
            'companyName': ssss,
            'gender': that.userGender,
            'birthday': that.userBirthday,
            'identity': that.userIdentity,
            'blacklist': that.blackListState
          },
          {
            emulateJSON: true
          }
        ).then(response => {
          console.log(response.json())
        })
      },
      getCompanies () {
        let that = this
        that.$http.get(
          'http://192.168.0.72:8080/api/v1/companyByAppKey',
          {
            'app_key': '1f462eb305a2417c9564f2dfaf89da9c'
          }
        ).then(response => {
          console.log(response.json())
        })
      },
//      changeValue (value) {
//
//      },
      handleAvatarSuccess (res, file) {
        this.userPhoto = URL.createObjectURL(file.raw)
      },
      beforeAvatarUpload (file) {
        const isJPG = file.type === 'image/jpeg'
        const isLt2M = file.size / 1024 / 1024 < 2

        if (!isJPG) {
          this.$message.error('上传头像图片只能是 JPG 格式!')
        }
        if (!isLt2M) {
          this.$message.error('上传头像图片大小不能超过 2MB!')
        }
        return isJPG && isLt2M
      }
    }
  }
</script>

<style>
  .avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #20a0ff;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
  }
  .avatar {
    width: 178px;
    height: 178px;
    display: block;
  }
</style>
