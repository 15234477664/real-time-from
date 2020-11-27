# real-time-from
vue项目表单自动保存（定时缓存）

```html
<template>
  <div class="center_box contract-add-box" id="contract_box">
      <div class="coninfor_box">
        <div class="coninfor_title">创建</div>
        <div class="coninfor_con">
          <div class="infor_side_conbigbox">
            <el-form :model="projectForm" :rules="rules" ref="projectForm" label-width="180px" class="demo-ruleForm">
              <div class="dialog_cancel_infoson_father">
                <div class="dialog_cancel_infoson_title">
                  <div class="dialog_cancel_infoson_titlewenzi">企业信息</div>
                  <div class="dialog_cancel_infoson_titlexian"></div>
                </div>
                <div class="dialog_detail">
                  <el-row>
                    <el-col :span="24">
                      <el-form-item label="企业名称：" prop="enterpriseName">
                        <el-input placeholder="请输入企业名称" v-model="projectForm.enterpriseName" :disabled="isEnterpriseId !== ''"></el-input>
                      </el-form-item>
                    </el-col>
                  </el-row>
                  <el-row :gutter="20">
                    <el-col :span="12">
                      <el-form-item label="注册地址：" prop="provinceId">
                        <el-cascader
                          :disabled="isEnterpriseId !== ''"
                          expand-trigger="hover"
                          :options="addressOptions"
                          v-model="projectForm.selectedcityOptions"
                          @change="handlecityChange">
                        </el-cascader>
                      </el-form-item>
                    </el-col>
                    <el-col :span="12">
                      <el-form-item prop="address" label-width="0">
                        <el-input placeholder="请输入详细地址" v-model="projectForm.address" :disabled="isEnterpriseId !== ''"></el-input>
                      </el-form-item>
                    </el-col>
                  </el-row>
                  <el-row>
                    <el-col :span="24">
                      <el-form-item label="所属行业：" prop="industryCategory">
                        <el-cascader
                          :disabled="isEnterpriseId !== ''"
                          class="industry_box"
                          expand-trigger="hover"
                          :options="industryOptions"
                          v-model="projectForm.selecIndustry"
                          @change="selectIndusChange">
                        </el-cascader>
                      </el-form-item>
                    </el-col>
                  </el-row>
                </div>
              </div>
              <div class="dialog_cancel_infoson_father">
                <div class="dialog_cancel_infoson_title">
                  <div class="dialog_cancel_infoson_titlewenzi">联系人信息</div>
                  <div class="dialog_cancel_infoson_titlexian"></div>
                </div>
                <div class="dialog_detail">
                  <el-row>
                    <el-col :span="24">
                      <el-form-item label="姓名：" prop="contactName">
                        <el-input placeholder="请输入姓名" v-model="projectForm.contactName" :disabled="isEnterpriseId !== ''"></el-input>
                      </el-form-item>
                    </el-col>
                    <el-col :span="24">
                      <el-form-item label="移动电话：" prop="contactPhone">
                        <el-input placeholder="请输入移动电话" v-model="projectForm.contactPhone" :disabled="isEnterpriseId !== ''"></el-input>
                      </el-form-item>
                    </el-col>
                    <el-col :span="24">
                      <el-form-item label="身份证号码：" prop="idCode">
                        <el-input placeholder="请输入身份证号码" v-model="projectForm.idCode" :disabled="isEnterpriseId !== ''"></el-input>
                      </el-form-item>
                    </el-col>
                    <el-col :span="24">
                      <el-form-item label="URL地址：" prop="urlLink">
                        <el-input placeholder="请输入URL地址" v-model="projectForm.urlLink" :disabled="isEnterpriseId !== ''"></el-input>
                      </el-form-item>
                    </el-col>
                    <el-col :span="24">
                      <el-form-item label="整数：" prop="integerNumber">
                        <el-input placeholder="请输入整数" v-model="projectForm.integerNumber" :disabled="isEnterpriseId !== ''"></el-input>
                      </el-form-item>
                    </el-col>
                    <el-col :span="24">
                      <el-form-item label="金额（保留两位小数）：" prop="money">
                        <el-input placeholder="请输入整数" v-model="projectForm.money" :disabled="isEnterpriseId !== ''"></el-input>
                      </el-form-item>
                    </el-col>
                    <el-col :span="24">
                      <el-form-item label="联系方式：" prop="contact">
                        <el-input placeholder="请输入整数" v-model="projectForm.contact" :disabled="isEnterpriseId !== ''"></el-input>
                      </el-form-item>
                    </el-col>
                    <el-col :span="24">
                      <el-form-item label="车牌号：" prop="license">
                        <el-input placeholder="请输入车牌号" v-model="projectForm.license" :disabled="isEnterpriseId !== ''"></el-input>
                      </el-form-item>
                    </el-col>
                    <el-col :span="24">
                      <el-form-item label="中文：" prop="chinese">
                        <el-input placeholder="请输入中文" v-model="projectForm.chinese" :disabled="isEnterpriseId !== ''"></el-input>
                      </el-form-item>
                    </el-col>
                    <el-col :span="24">
                      <el-form-item label="电子邮箱：" prop="contactEmail">
                        <el-input placeholder="请输入电子邮箱" v-model="projectForm.contactEmail" :disabled="isEnterpriseId !== ''"></el-input>
                      </el-form-item>
                    </el-col>
                  </el-row>
                </div>
              </div>
              <el-form-item class="operation_btn_box" v-if="!isEnterpriseId">
                <el-button type="primary" @click="submitForm('projectForm')" class="save_btn" :loading="subStatus">提交</el-button>
                <el-button @click="$router.go(-1)" class="cancel_btn" :loading="subStatus">取消</el-button>
              </el-form-item>
            </el-form>
          </div>
        </div>
      </div>
  </div>
</template>
```
```js
<script>
import {isvalidPhone, isvalidId, links, integerNumber, moneyType, isvalidFixedPhone, licenseNumber, chineseType} from '@/assets/js/validate'
import * as cityData from '@/assets/js/region'
import * as induData from '@/assets/js/industry'
import uploadFile from '@/components/upload/publicFileUpload'
export default {
  name: '',
  components: {
    uploadFile
  },
  data () {
    // 手机号
    var validNumber = (rule, value, callback) => {
      if (!value) {
        callback(new Error('请输入手机号码'))
      } else if (!isvalidPhone(value)) {
        callback(new Error('请输入正确的格式'))
      } else {
        callback()
      }
    }
    // 身份证
    var validIdCode = (rule, value, callback) => {
      if (!value) {
        callback()
      } else if (!isvalidId(value)) {
        callback(new Error('请输入正确的格式'))
      } else {
        callback()
      }
    }
    // 链接地址
    var validLinks = (rule, value, callback) => {
      if (!value) {
        callback()
      } else if (!links(value)) {
        callback(new Error('请输入正确的格式'))
      } else {
        callback()
      }
    }
    // 整数
    var validIntegerNumber = (rule, value, callback) => {
      if (!value) {
        callback()
      } else if (!integerNumber(value)) {
        callback(new Error('请输入正确的格式'))
      } else {
        callback()
      }
    }
    // 金额
    var validMoney = (rule, value, callback) => {
      if (!value) {
        callback()
      } else if (!moneyType(value)) {
        callback(new Error('请输入正确的格式'))
      } else {
        callback()
      }
    }
    // 联系方式
    var validContact = (rule, value, callback) => {
      if (!value) {
        callback()
      } else if (!isvalidFixedPhone(value)) {
        callback(new Error('请输入正确的格式'))
      } else {
        callback()
      }
    }
    // 车牌号验证
    var validLicense = (rule, value, callback) => {
      if (!value) {
        callback()
      } else if (!licenseNumber(value)) {
        callback(new Error('请输入正确的格式'))
      } else {
        callback()
      }
    }
    // 中文验证
    var validChinese = (rule, value, callback) => {
      if (!value) {
        callback()
      } else if (!chineseType(value)) {
        callback(new Error('请输入正确的格式'))
      } else {
        callback()
      }
    }
    return {
      isEnterpriseId: '',
      projectForm: {}, // 企业信息
      subStatus: false, // 是否保存
      // 表单验证
      rules: {
        enterpriseName: [
          { required: true, message: '请输入企业名称', trigger: ['blur', 'change'] },
          {min: 1, max: 200, message: '长度在 1 到 200 个字符', trigger: ['blur', 'change']}
        ],
        provinceId: [
          { required: true, message: '请选择注册地址', trigger: ['blur', 'change'] }
        ],
        address: [
          { required: true, message: '请输入详细地址', trigger: ['blur', 'change'] },
          {min: 1, max: 200, message: '长度在 1 到 200 个字符', trigger: ['blur', 'change']}
        ],
        industryCategory: [
          { required: true, message: '请选择所属行业', trigger: ['blur', 'change'] }
        ],
        contactName: [
          { required: true, message: '请输入姓名', trigger: ['blur', 'change'] },
          {min: 1, max: 20, message: '长度在 1 到 20 个字符', trigger: ['blur', 'change']}
        ],
        contactPhone: [
          { required: true, message: '请输入手机号', trigger: ['blur', 'change'] },
          { trigger: ['blur', 'change'], validator: validNumber }
        ],
        idCode: [
          { required: false, message: '请输入身份证号码', trigger: ['blur', 'change'] },
          { trigger: ['blur', 'change'], validator: validIdCode }
        ],
        urlLink: [
          { required: false, message: '请输入URL地址', trigger: ['blur', 'change'] },
          { trigger: ['blur', 'change'], validator: validLinks }
        ],
        integerNumber: [
          { required: false, message: '请输入整数', trigger: ['blur', 'change'] },
          { trigger: ['blur', 'change'], validator: validIntegerNumber }
        ],
        money: [
          { required: false, message: '请输入整数', trigger: ['blur', 'change'] },
          { trigger: ['blur', 'change'], validator: validMoney }
        ],
        contact: [
          { required: false, message: '请输入联系方式', trigger: ['blur', 'change'] },
          { trigger: ['blur', 'change'], validator: validContact }
        ],
        license: [
          { required: false, message: '请输入联系方式', trigger: ['blur', 'change'] },
          { trigger: ['blur', 'change'], validator: validLicense }
        ],
        chinese: [
          { required: false, message: '请输入联系方式', trigger: ['blur', 'change'] },
          { trigger: ['blur', 'change'], validator: validChinese }
        ],
        contactEmail: [
          { required: false, message: '请输入邮箱地址', trigger: 'blur' },
          {min: 1, max: 20, message: '长度在 1 到 20 个字符', trigger: ['blur', 'change']},
          { type: 'email', message: '请输入正确的邮箱地址', trigger: ['blur', 'change'] }
        ],
        contractRules: [
          { required: true, message: '请输入合同名称', trigger: ['blur', 'change'] },
          {min: 1, max: 200, message: '长度在 1 到 200 个字符', trigger: ['blur', 'change']}
        ],
        contractNumberRules: [
          { required: true, message: '请输入合同编号', trigger: ['blur', 'change'] },
          {min: 1, max: 200, message: '长度在 1 到 200 个字符', trigger: ['blur', 'change']}
        ]
      },
      // 三级联动
      addressOptions: cityData.CityInfo,
      // 所属行业
      industryOptions: induData.industry
    }
  },
  methods: {
    // 将数据缓存到本地的方法
    setLocal () {
      let localData = this.projectForm
      let valuesStr = JSON.stringify(localData)
      window.localStorage.setItem('recordData',valuesStr)
      // this.$message.success('保存成功！');
    },
    initFrom () {
      if(window.localStorage.getItem('recordData')) {
        let getLocal = JSON.parse(window.localStorage.getItem('recordData'))
        console.log(getLocal)
        this.projectForm = getLocal
      }
    },
    // 添加企业信息
    submitForm (form) {
      this.$refs[form].validate((valid) => {
        if (valid) {
          console.log(this.projectForm)
        } else {
          return false
        }
      })
    },
    // 三级联动选择
    handlecityChange (value) {
      this.projectForm.provinceId = value[0]
      this.projectForm.cityId = value[1]
      this.projectForm.countyId = value[2]
    },
    selectIndusChange () {
      let industry = this.selecIndustry.toString().split(',')
      this.projectForm.industryCategory = industry[0]
      this.projectForm.industryClass = industry[1]
    }
  },
  watch: {
    projectForm:{
      //注意：当观察的数据为对象或数组时，curVal和oldVal是相等的，因为这两个形参指向的是同一个数据对象
      handler(curVal,oldVal){
        // 自动保存方法
        console.log(123)
        this.setLocal()
      },
      deep:true
    }
  },
  mounted () {
    this.initFrom()
  }
}
</script>
```
```css
<style scoped lang="less">
  .contract-add-box{
    .industry_box{
      width: 100%;
    }
    .contract_bigbox{
      width: 100%;
    }
    .shanchun_icon{
      img{
        width: 20px;
        height: 20px;
      }
    }
  }
</style>
<style lang="less">
.contract-add-box{
  .el-date-editor.el-range-editor{
    .el-range-input{
      height: 30px;
    }
  }
}
.el-cascader-panel {
  display: block;
  .el-scrollbar {
    float: left;
  }
}
</style>
```
