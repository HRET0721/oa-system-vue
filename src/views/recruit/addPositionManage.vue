<template>
  <div>
    <h1>职位信息</h1>
    职位名称:&nbsp;
    <el-input
      placeholder="请输入内容"
      v-model="addPostData.jobName"
      clearable
      style="width: 350px; height: 40px"
    ></el-input
    >招聘人数:&nbsp;<el-input
      placeholder="请输入内容"
      v-model="addPostData.jobNumber"
      clearable
      style="width: 350px"
    ></el-input
    ><br /><br />
    职位性质:
    <el-select
      v-model="addPostData.jobNature"
      placeholder="全部"
      style="width: 350px"
    >
      <el-option label="全职" value="1"></el-option>
      <el-option label="兼职" value="2"></el-option>
      <el-option label="实习" value="3"></el-option>
      <el-option label="外派" value="4"></el-option>
      <el-option label="退休返聘" value="5"></el-option>
    </el-select>
    用人部门:
    <el-select
      v-model="addPostData.jobDept"
      placeholder="全部"
      style="width: 350px; height: 40px"
    >
      <el-option label="技术部" value="1"></el-option>
      <el-option label="产品部" value="2"></el-option>
      <el-option label="销售部" value="3"></el-option>
      <el-option label="行政部" value="4"></el-option>
    </el-select>
    <br /><br />
    工作地点:&nbsp;<el-input
      placeholder="请输入内容"
      v-model="addPostData.jobAddress"
      clearable
      style="width: 350px; height: 40px"
    ></el-input>
    详细地址:&nbsp;<el-input
      placeholder="请输入内容"
      v-model="addPostData.jobDetailedAddress"
      clearable
      style="width: 350px; height: 40px"
    ></el-input>
    <h1>招聘要求</h1>
    薪资范围:<el-input
      placeholder=""
      v-model="addPostData.jobMinPay"
      clearable
      style="width: 100px; height: 40px"
    ></el-input
    >-
    <el-input
      placeholder=""
      v-model="addPostData.jobMaxPay"
      clearable
      style="width: 100px; height: 40px"
    ></el-input
    >&nbsp;
    <el-input
      placeholder=""
      v-model="addPostData.jobNumPay"
      clearable
      style="width: 100px; height: 40px"
    ></el-input
    >薪&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;学历要求:
    <el-select
      v-model="addPostData.jobEducation"
      placeholder="全部"
      style="width: 350px; height: 40px"
    >
      <el-option label="不限" value="1"></el-option>
      <el-option label="高中及以下" value="2"></el-option>
      <el-option label="大专" value="3"></el-option>
      <el-option label="本科" value="4"></el-option>
      <el-option label="硕士" value="5"></el-option>
      <el-option label="博士及以上" value="6"></el-option> </el-select
    ><br /><br />
    工作经验:<el-input
      placeholder="请输入内容"
      v-model="addPostData.jobExperience"
      clearable
      style="width: 350px; height: 40px"
    ></el-input>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 专业:<el-input
      placeholder="请输入内容"
      v-model="addPostData.jobMajor"
      clearable
      style="width: 350px; height: 40px"
    ></el-input>
    <br />
    <br />
    职位描述:<el-input
      :rows="1"
      placeholder="请输入内容"
      v-model="addPostData.jobDescribe"
      style="width: 1000px; height: 90px"
    ></el-input>

    <!-- <img
      v-if="addPostData.jobFile != ''"
      :src="addPostData.jobFile"
      style="width: 100px; height: 100px"
    /> -->
    <br />
    <br />

    <el-upload
      v-model:file-list="fileList"
      class="avatar-uploader"
      action="/recruitJob/userImg"
      multiple
      :on-preview="handlePreview"
      :on-remove="handleRemove"
      :before-remove="beforeRemove"
      :limit="3"
      :on-exceed="handleExceed"
    >
      上传文件：
      <el-button type="primary">图片上传</el-button>
      &nbsp;&nbsp;&nbsp;
      <div class="el-upload__tip">
        支持扩展名：.png .gif .pdf .jpg,单个文件不超过500kb。
      </div>
    </el-upload>

    <div style="margin-left: 1200px; margin-top: 100px">
      <el-button style="width: 125px" @click="returnq">取消</el-button>
      <el-button
        v-if="addPostData != null"
        style="width: 125px; color: aliceblue"
        type="primary"
        @click="addPostDataMethod()"
        >下一步</el-button
      >
      <el-button
        v-else
        style="width: 125px"
        type="primary"
        @click="updatePost()"
        >修改</el-button
      >
    </div>
    <br />
    <!-- <el-button
      v-if="updateflag == 'true'"
      size="small"
      type="primary"
      @click="findById"
      >下一步</el-button
    > -->
  </div>
</template>

<script>
import axios from 'axios'
import { ElMessage } from 'element-plus'

export default {
  data() {
    return {
      detailsDate: this.$route.query.detailsDate,
      updateflag: this.$route.query.updateflag,
      fileList: [],
      JobId: '',
      addPostData: {
        jobId: '', // 职位id
        jobName: '', //职位名称
        jobNumber: '', //招聘人数
        jobStatus: '', // 职位状态
        jobDept: '', // 部门
        jobAddress: '', // 工作地点
        jobDetailedAddress: '', // 详细地址
        jobMinPay: '', // 最低薪资
        jobMaxPay: '', // 最高薪资
        jobNumPay: '', // 薪资
        jobEducation: '', // 学历要求
        jobExperience: '', // 工作经验
        jobMajor: '', // 专业
        jobDescribe: '', // 职位描述
        jobFile: '', // 文件
        jobPrincipal: '', // 招聘负责人
        jobNature: '' // 职位性质
      }
    }
  },
  methods: {
    // 取消
    returnq() {
      this.$router.push('/home/recruit/post')
    },
    // 上传文件
    handleRemove(file, fileList) {
      console.log(file, fileList)
    },
    // 文件上传
    handlePreview(file) {
      console.log(file)
    },
    // 文件上传
    handleAvatarSuccess(res, file) {
      console.log(file)
      this.addPostData.jobFile = res.data
    },
    // 添加职位信息
    addPostDataMethod() {
      console.log('添加职位信息')
      axios.post('/recruitJob/addJob', this.addPostData).then((result) => {
        if (result.data.code == 200) {
          console.log(result)
          ElMessage.success('恭喜你，添加成功')
          this.$router.push('/home/recruit/setPositionManage')
        } else {
          ElMessage.error('添加职位信息失败')
        }
      })
    },
    findById() {
      axios
        .post('/recruitJob/findById?JobId=' + this.$route.query.JobId)
        .then((res) => {
          console.log(res)
          this.addPostData = res.data
          console.log(this.addPostData)
        })
    },
    updatePost() {
      console.log('修改职位信息')
      axios.post('/recruitJob/updateJob', this.addPostData).then((res) => {
        if (res.data.code == 200) {
          console.log('修改成功')
          ElMessage.success('修改成功')
          this.$router.push('/home/recruit/setPositionManage')
        } else {
          ElMessage.error('修改失败')
        }
      })
    }
  },
  // 添加职位信息
  mounted() {
    this.findById()
  }
}
</script>

<style></style>
