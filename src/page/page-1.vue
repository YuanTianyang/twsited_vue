<template>
  <el-form >
    <el-form-item label-width="300px" label="公司名称">
      <el-input v-model="companyName" placeholder="请输入公司名称"></el-input>
    </el-form-item>
    <el-form-item label-width="300px" label="公司地址">
      <el-input v-model="address" placeholder="请输入公司地址"></el-input>
    </el-form-item>
    <el-form-item label-width="300px" label="公司简称">
      <el-input v-model="companyShortName" placeholder="请输入公司简称"></el-input>
    </el-form-item>
    <el-form-item label-width="300px" label="国家名称">
      <el-input v-model="country" placeholder="请输入国家"></el-input>
    </el-form-item>
    <el-form-item label-width="300px" label="省份名称">
      <el-input v-model="province" placeholder="请输入省份"></el-input>
    </el-form-item>
    <el-form-item label-width="300px" label="城市名称">
      <el-input v-model="city" placeholder="请输入城市"></el-input>
    </el-form-item>
    <el-form-item label-width="300px" label="负责人">
      <el-input v-model="manager" placeholder="请输入负责人"></el-input>
    </el-form-item>
    <el-form-item label-width="300px" label="公司地址经度">
      <el-input v-model="latitude" placeholder="请输入内容"></el-input>
    </el-form-item>
    <el-form-item label-width="300px" label="公司地址纬度">
      <el-input v-model="longtitude" placeholder="请输入内容"></el-input>
    </el-form-item>
    <el-form-item label-width="300px" label="公司电话">
      <el-input v-model="telephone" placeholder="请输入内容"></el-input>
    </el-form-item>
    <el-form-item label-width="300px" label="公司网址">
      <el-input v-model="companyWebUrl" placeholder="请输入内容"></el-input>
    </el-form-item>
    <el-form-item label-width="300px" label="手机号码">
      <el-input v-model="mobilePhone" placeholder="请输入内容"></el-input>
    </el-form-item>
    <el-form-item label-width="300px" label="电子邮箱">
      <el-input v-model="mailingAddress" placeholder="请输入内容"></el-input>
    </el-form-item>
    <el-form-item label-width="300px" label="logo">
      <el-input v-model="logo" placeholder="请输入内容"></el-input>
    </el-form-item>
    <el-form-item  label-width="300px">
      <el-button type="info" @click="createCompany()">确认</el-button>
      <el-button :plain="true">取消</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
  export default {
    data () {
      return {
        companyName: '',
        address: '',
        companyShortName: '',
        country: '',
        province: '',
        city: '',
        manager: '',
        latitude: '',
        longtitude: '',
        telephone: '',
        companyWebUrl: '',
        mobilePhone: '',
        mailingAddress: '',
        logo: ''
      }
    },
    methods: {
      createCompany () {
        let that = this
        that.$http.post(
          '/v1/company',
          {
            companyName: that.companyName,
            address: that.address,
            companyShortName: that.companyShortName,
            country: that.country,
            province: that.province,
            city: that.city,
            manager: that.manager,
            latitude: that.latitude,
            longtitude: that.longtitude,
            telephone: that.telephone,
            companyWebUrl: that.companyWebUrl,
            mobilePhone: that.mobilePhone,
            mailingAddress: that.mailingAddress,
            logo: that.logo,
            appKey: '123123124'
          },
          {
            emulateJSON: true
          }
        ).then(response => {
          console.log(response.json())
        })
      }
    }
  }
</script>
