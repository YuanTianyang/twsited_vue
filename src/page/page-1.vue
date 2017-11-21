<template>
  <el-form>
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
      <el-input v-model="longitude" placeholder="请输入内容"></el-input>
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
      <el-input v-model="version" placeholder="请输入内容" style="display: none"></el-input>
    </el-form-item>
    <el-form-item label-width="300px">
      <el-button type="info" @click="createOrUpdateCompany()">确认</el-button>
      <el-button :plain="true">取消</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
  export default {
    data () {
      return {
        id: '',
        companyName: '',
        address: '',
        companyShortName: '',
        country: '',
        province: '',
        city: '',
        manager: '',
        latitude: '',
        longitude: '',
        telephone: '',
        companyWebUrl: '',
        mobilePhone: '',
        mailingAddress: '',
        logo: '',
        version: ''
        /*
        that.Data = company 这种写法可以在返回值时直接
        Data: {
          companyName: '',
          address: '',
          companyShortName: '',
          country: '',
          province: '',
          city: '',
          manager: '',
          latitude: '',
          longitude: '',
          telephone: '',
          companyWebUrl: '',
          mobilePhone: '',
          mailingAddress: '',
          logo: ''
        }
         */
      }
    },
    mounted () {
      let that = this
      const id = that.$route.query.id
      if (id) {
        alert('当前为公司修改页面')
        console.log(id)
        that.id = id
        that.getCompany(id)
      } else {
        alert('当前为公司新增页面')
      }
    },
    methods: {
      createOrUpdateCompany () {
        let that = this
        if (that.id) {
          alert('修改公司')
          that.$http.put(
            '/v1/company',
            {
              id: that.id,
              companyName: that.companyName,
              address: that.address,
              companyShortName: that.companyShortName,
              country: that.country,
              province: that.province,
              city: that.city,
              manager: that.manager,
              latitude: that.latitude,
              longitude: that.longitude,
              telephone: that.telephone,
              companyWebUrl: that.companyWebUrl,
              mobilePhone: that.mobilePhone,
              mailingAddress: that.mailingAddress,
              logo: that.logo,
              version: that.version,
              appKey: '123123124'
            },
            {
              emulateJSON: true
            }
          ).then(response => {
            console.log(response.json())
          })
        } else {
          alert('创建公司')
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
              longitude: that.longitude,
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
      },
      getCompany (id) {
        let that = this
        that.$http.get(
          '/v1/company/' + id,
          {
            emulateJSON: true
          }
        ).then(response => {
          let that = this
          let data = response.data
          if (data.code > 0) {
            let company = data.data
            console.log(company)
            that.companyName = company.companyName
            that.address = company.address
            that.companyShortName = company.companyShortName
            that.country = company.country
            that.province = company.province
            that.city = company.city
            that.manager = company.manager
            that.latitude = company.latitude
            that.longitude = company.longitude
            that.telephone = company.telephone
            that.companyWebUrl = company.companyWebUrl
            that.mobilePhone = company.mobilePhone
            that.mailingAddress = company.mailingAddress
            that.logo = company.logo
            that.version = company.version
          } else {
            alert('失败:' + data.msg)
          }
        })
      }
    }
  }
</script>
