<template>
  <div>
    <!-- <el-upload
      class="upload-demo"
      action="http://m.rcfortress.site:7899/uploadFile"
      :on-preview="handlePreview"
      :on-remove="handleRemove"
      :before-remove="beforeRemove"
      multiple
      :limit="3"
      :on-exceed="handleExceed"
      :file-list="fileList"
    >
      <div class="uploadContainer">
        <img :src="showPic" />
        <el-button type="primary" style="margin-top: 30px">点击上传</el-button>
      </div>
    </el-upload> -->
    <el-upload
      class="upload-demo"
      action="http://m.rcfortress.site:7899/uploadFile"
      method="post"
      accept=".jpg,.png,.ico,.gif"
      :show-file-list="false"
      :on-change="doUpload"
      :limit="1"
      :on-exceed="handleExceed"
      :file-list="fileList"
      :before-upload="beforeUpload"
    >
      
      <template #trigger>
        <el-button type="primary">Click to upload</el-button>
      </template>
      <template #default>
        <img :src="showPic" style="height: 100px; width: 100px" />
      </template>
      <template #tip>
        <div class="el-upload__tip">只允许jpg/png/ico/gif</div>
      </template>
    </el-upload>
  </div>
</template>

<script>
import { ElMessage, ElMessageBox } from "element-plus";

import { UploadProps, UploadUserFile } from "element-plus";
export default {
  name: "uploadCom",
  components: {
    UploadProps,
    UploadUserFile,
    ElMessage,
    ElMessageBox,
  },
  props: {},
  data() {
    return {
      showPic: "http://m.rcfortress.site:7899/static/add.png",
      remoteAddr: "",
      fileList: [],
    };
  },
  mounted() {},
  methods: {
    doUpload() {
      console.log("执行上传");
      this.$data.remoteAddr =
        "http://m.rcfortress.site:7899" + this.$data.fileList[0].response;
      console.log(this.$data.remoteAddr);
      this.$data.showPic = this.$data.remoteAddr
    },
    beforeUpload() {
      console.log("准备上传");
    },
  },
};
</script>
<style>
.editor-slide-upload .el-upload {
  text-align: left;
}
.uploadContainer {
  transform: translate(40px, 0px);
  width: 120px;
  height: 120px;
  display: flex;
  flex-direction: column;
}
</style>

