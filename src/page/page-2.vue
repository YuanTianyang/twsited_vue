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
          <img v-if="imageUrl" :src="imageUrl" class="avatar">
          <i v-else class="el-icon-plus avatar-uploader-icon"></i>
          </el-upload>
        </el-form-item>
        <el-form-item label-width="300px" label="用户名称">
          <el-input v-model="input" placeholder="请输入用户昵称"></el-input>
        </el-form-item>
        <el-form-item label-width="300px" label="手机号">
          <el-input v-model="input" placeholder="请输入手机号"></el-input>
        </el-form-item>
        <el-form-item label-width="300px" label="所属公司">
          <el-select v-model="value" filterable  placeholder="请选择">
            <el-option
              v-for="item in companys"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label-width="300px" label="性别">
          <el-radio class="radio" v-model="radio" label="1">男</el-radio>
          <el-radio class="radio" v-model="radio" label="2">女</el-radio>
        </el-form-item>
        <el-form-item label-width="300px" label="婚否">
          <el-switch
            v-model="isMarry"
            on-text="已婚"
            off-text="未婚"
            on-color="#13ce66"
            off-color="#ff4949">
          </el-switch>
        </el-form-item>
        <el-form-item label-width="300px" label="生日">
          <el-date-picker
            v-model="value1"
            type="date"
            placeholder="选择日期"
            :picker-options="pickerOptions0">
          </el-date-picker>
        </el-form-item>
        <el-form-item label-width="300px" label="兴趣爱好">
          <el-tag
            :key="tag"
            v-for="tag in dynamicTags"
            :closable="true"
            :close-transition="false"
            @close="handleClose(tag)"
          >
            {{tag}}
          </el-tag>
          <el-input
            class="input-new-tag"
            v-if="inputVisible"
            v-model="inputValue"
            ref="saveTagInput"
            size="mini"
            @keyup.enter.native="handleInputConfirm"
            @blur="handleInputConfirm"
          >
          </el-input>
        </el-form-item>
        <el-form-item label-width="300px" label="身份证号">
          <el-input v-model="input" placeholder="请输入身份证号"></el-input>
        </el-form-item>
        <el-form-item label-width="300px">
          <el-button type="primary">确认</el-button>
          <el-button>取消</el-button>
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
        radio: '1',
        pickerOptions0: {
          disabledDate (time) {
            return time.getTime() > Date.now() - 8.64e7
          }
        },
        value1: '',
        companys: [{
          value: '1',
          label: '滴滴'
        }, {
          value: '2',
          label: '神州专车'
        }, {
          value: '3',
          label: '易到用车'
        }, {
          value: '4',
          label: '小咖出行'
        }, {
          value: '5',
          label: '哒哒速运'
        }],
        value: '',
        isMarry: true,
        dynamicTags: ['标签一', '标签二', '标签三'],
        inputVisible: false,
        inputValue: ''

      }
    },
    computed: {
    },
    methods: {
      handleAvatarSuccess (res, file) {
        this.imageUrl = URL.createObjectURL(file.raw)
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
      },
      handleClose (tag) {
        this.dynamicTags.splice(this.dynamicTags.indexOf(tag), 1);
      },
      showInput () {
        this.inputVisible = true
        this.$nextTick(_ => {
          this.$refs.saveTagInput.$refs.input.focus()
        })
      },
      handleInputConfirm () {
        let inputValue = this.inputValue
        if (inputValue) {
          this.dynamicTags.push(inputValue)
        }
        this.inputVisible = false
        this.inputValue = ''
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
