
<template>
  <div class="content" v-if="reloadFlag">
    <div>
      <img
        src="../../static/logo.png"
        class="logo"
        v-if="status"
        @click="changeStatus"
      />
      <div v-if="!status">
        <div>
          <img
            src="../../static/long.png"
            class="long"
            @click="changeStatus()"
          />
        </div>
        <div>喔唷，崩溃啦！</div>
        <div @click="toUpload">显示 你妈 时出了些问题。</div>
        <div style="font-weight: 700">可能是你妈不存在</div>
        <div style="color: #007aff" @click="navigate">了解详情</div>
      </div>
    </div>
    <div class="search-bar">
      <input
        class="search-bar-input"
        auto-focus="autofocus"
        placeholder="有病一下 你就知道"
        placeholder-style="font-family: 'Jetbrains Mono';"
        v-model="searchText"
        @input="searchLog"
        @confirm="searchBing()"
      />
    </div>
    <div class="buttonBox">
      <button class="search-bar-button" style="" @click="searchBing">
        Bing
      </button>
      <button class="search-bar-button" @click="searchBaidu">Baidu</button>
    </div>
    <div class="nav-panel">
      <urlItem
        v-for="content in sites"
        v-bind:content="content"
        v-bind:key="content.url"
        @delete="deleteSite"
      ></urlItem>
      <addSite @addPage="updateSite"></addSite>
      <!-- <div class="nav-panel-item" title="Bilibili" href="https://www.bilibili.com/" target="_blank">
			        <img src="https://www.bilibili.com/favicon.ico" alt="Bilibili" height="16" width="16" class="nav-panel-item-img">
			        <div class="nav-panel-item-txt">Bilibili</div>
			    </div> -->
      <!-- <addPage></addPage> -->
    </div>
    <el-dialog
      v-model="dialogVisible"
      title="添加站点"
      width="40%"
      :before-close="handleClose"
      style="min-width: 400px"
    >
      <div class="uploadBox">
        <div style="margin: 40px">
          <div style="height: 20px"></div>
          <el-input v-model="inputName" placeholder="站点名" />
          <div style="height: 20px"></div>
          <el-input v-model="inputUrl" placeholder="站点地址" />
          <div style="height: 20px"></div>
        </div>
        <div style="margin: 40px">
          <div class="fixP">
            <upload-com @uploadComplete="handlePic"> </upload-com>
          </div>
          <!-- <div>
            <img :src="uploadLogo" style="width: 100px; height: 100px" />
          </div>
          <div>
            <el-button type="primary" @click="uploadPic">上传图标</el-button>
          </div> -->
        </div>
      </div>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogVisible = false">取消</el-button>
          <el-button type="primary" @click="confirmUpload">确认</el-button>
        </span>
      </template>
    </el-dialog>
    <el-dialog
      v-model="deleteVis"
      title="确认"
      width="30%"
      :before-close="handleClose"
    >
      <span>确定要删除此项目吗</span>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="deleteVis = false">取消</el-button>
          <el-button type="primary" @click="confirmDelete">确认</el-button>
        </span>
      </template>
    </el-dialog>
    <div style="text-align: center;color: #989898;margin-top: 30px;">
			<div style="padding-top: 10px;">Ybbgle Remastered</div>
			<div style="padding-top: 10px;">Copyright © 2022 PRC, HDU, Inistitude Of Automation.</div>
			<div style="padding-top: 10px;">All Rights Reserved.</div>
		</div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import {
  ElMessageBox,
  UploadProps,
  UploadUserFile,
  ElMessage,
} from "element-plus";

import urlItem from "@/components/urlItem.vue";
import addSite from "@/components/addSite.vue";
import uploadCom from "@/components/uploadCom.vue";
// import * as echarts from "echarts"
import axios from "axios";
export default {
  name: "HomeView",
  components: {
    ElMessage,
    urlItem,
    addSite,
    ElMessageBox,
    UploadProps,
    UploadUserFile,
    uploadCom,
  },
  data() {
    return {
      title: "Hello",
      searchText: "",
      sites: null,
      status: true,
      reloadFlag: false,
      dialogVisible: false,
      uploadLogo: "",
      inputName: "",
      inputUrl: "",
      deleteVis: false,
      deleteTitle: "",
    };
  },
  mounted() {
    let _that = this;
    _that.$data.reloadFlag = false;
    _that.getData();
    // axios({
    //   method: "get",
    //   url: "http://m.rcfortress.site:7899/navi/getSiteList",
    // }).then(function (res) {
    //   console.log("已经获取网站列表");
    //   console.log(res.data);
    //   _that.$data.sites = res.data;
    //   _that.$data.reloadFlag = true;
    // });
    document.addEventListener("keydown", function (e) {
      if (e.keyCode == 13) {
        _that.searchBing();
      }
    });
    // uni.request({
    //   url: "http://m.rcfortress.site:7899/navi/getSiteList",
    //   method: "GET",
    //   success(res) {
    //     console.log("已经获取网站列表");
    //     console.log(res.data);
    //     _that.$data.sites = res.data;
    //     _that.$data.reloadFlag = true;
    //   },
    // });
  },
  methods: {
    deleteSite(data) {
      this.$data.deleteVis = true;
      console.log("准备删除站点");
      console.log(data);
      this.$data.deleteTitle = data;
    },
    confirmDelete() {
      let _that = this;
      if (_that.$data.deleteTitle != "*") {
        axios({
          method: "delete",
          url: "http://m.rcfortress.site:7899/navi/deleteSite",
          data: {
            title: _that.$data.deleteTitle,
          },
        }).then(function (res) {
          console.log("已经删除站点");
          console.log(res);
          ElMessage("删除成功");
          _that.getData();
          setTimeout(function () {
            _that.$data.deleteVis = false;
          }, 300);
        });
      }
    },
    getData() {
      let _that = this;

      axios({
        method: "get",
        url: "http://m.rcfortress.site:7899/navi/getSiteList",
      }).then(function (res) {
        console.log("已经获取网站列表");
        console.log(res.data);
        _that.$data.sites = res.data;
        _that.$data.reloadFlag = true;
      });
    },
    handlePic(data) {
      console.log("接收到icon");
      console.log(data);
      this.$data.uploadLogo = data;
    },
    changeStatus() {
      this.$data.status = !this.$data.status;
    },
    updateSite() {
      console.log("弹出框体");
      this.$data.dialogVisible = true;
    },
    // changeMode() {
    //   uni.redirectTo({
    //     url: "./index",
    //   });
    // },
    // toUpload() {
    //   uni.navigateTo({
    //     url: "../upload/upload",
    //   });
    // },
    // navigate() {
    //   console.log("sb");
    //   uni.redirectTo({
    //     url: "./long/long",
    //   });
    // },
    searchLog() {
      console.log(this.$data.searchText);
    },
    searchBing() {
      let searchUrl = "https://cn.bing.com/search?q=" + this.$data.searchText;
      window.open(searchUrl);
    },
    searchBaidu() {
      let searchUrl = "https://www.baidu.com/s?wd=" + this.$data.searchText;
      window.open(searchUrl);
    },
    confirmUpload() {
      console.log(this.$data.inputName);
      console.log(this.$data.inputUrl);
      if (this.$data.inputName != "" && this.$data.inputUrl) {
        console.log("内容非空,允许上传");
        let _that = this;
        axios({
          method: "post",
          url: "http://m.rcfortress.site:7899/navi/updateSiteList",
          data: {
            url: _that.$data.inputUrl,
            title: _that.$data.inputName,
            icon: _that.$data.uploadLogo,
          },
        }).then(function (res) {
          console.log("更新上传成功");
          console.log(res);
          ElMessage("更新上传成功");
          _that.getData();
          setTimeout(function () {
            _that.$data.dialogVisible = false;
          }, 300);
        });
        // uni.request({
        //   url: "http://m.rcfortress.site:7899/navi/updateSiteList",
        //   method: "POST",
        //   data: {
        //     url: this.$data.websiteURL,
        //     title: this.$data.websiteTitle,
        //     icon: this.$data.picPath,
        //   },
        //   success(res) {
        //     console.log(res);
        //     uni.showModal({
        //       title: "提示",
        //       content: "记录已经上传,请回首页刷新查看",
        //       showCancel: false,
        //       success: function (res) {
        //         if (res.confirm) {
        //           console.log("UserConfirm");
        //         }
        //       },
        //     });
        //   },
        // });
      }
    },
  },
};
</script>
<style>
page {
  background-color: #ffffff;
  background-image: url(../../static/bg.svg);
  background-repeat: no-repeat;
  background-position: bottom;
  height: 100%;
}
.uploadBox {
  display: flex;
  flex-direction: row;
  justify-content: center;
}

.long {
  width: 100rpx;
  height: 100rpx;
}

.buttonBox {
  margin-top: 20rpx;
  margin-bottom: 40rpx;
}

.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  /* font-family: 'Jetbrains Mono'; */
}

.logo {
  width: 280px;
  height: 100px;
  margin-top: 20px;
}

.search-bar {
  margin-top: 25px;
  width: 600px;
  height: 100px;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: -50px;
}

.bg {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: -1;
}

.search-bar-input {
  width: 580px;
  height: 25px;
  padding: 9px 10px;
  font-family: "Microsoft YaHei UI", arial, sans-serif;
  font-size: 15px;
  color: #555;
  border: none;
  background: #fff;
  overflow: hidden;
  outline: none;
  vertical-align: middle;
  border-radius: 2px;
  background-color: #ffffff;
  box-shadow: 2px 5px 5px 2px rgba(0, 0, 0, 0.1);
}

.search-bar-button {
  margin-top: 20px;
  margin-left: 20px;
  width: 105px;
  height: 35px;
  line-height: 35px;
  overflow: hidden;
  text-align: center;
  font-size: 15px;
  font-weight: bold;
  color: rgb(255, 255, 255);
  border: none;
  overflow: hidden;
  vertical-align: middle;
  outline: none;
  cursor: pointer;
  border-radius: 5px;
  background-color: rgb(20, 162, 245);
  display: inline-block;

  /* margin-top: 20px;
		margin-left: 20px;
		width: 105px;
		height: 35px;
		line-height: 35px;
		overflow: hidden;
		text-align: center;
		font-size: 15px;
		font-weight: bold;
		color: #007AFF;
		border-color: #007AFF;
		border:1px solid;
		overflow: hidden;
		vertical-align: middle;
		outline: none;
		cursor: pointer;
		border-radius: 5px;
		background-color: #FFFFFF;
		display: inline-block; */
}

.search-bar-button:hover {
  background-color: rgb(30, 172, 245);
  box-shadow: 0px 0px 3px rgba(242, 242, 243, 0.6);
}

.nav-panel {
  backdrop-filter: blur(6px);
  opacity: 0.6;
  align-content: flex-start;
  margin-top: 20px;
  margin-bottom: 100px;
  width: 80%;
  min-height: 300px;
  max-width: 750px;
  margin-left: auto;
  margin-right: auto;
  padding-left: 30px;
  padding-top: 10px;
  /* background-color: #FFFFFF; */
  border-radius: 10px;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: flex-start;
  gap: 10rpx;
  transform: translateX(15px);
  /* box-shadow: 2px 5px 5px 5px rgba(0, 0, 0, 0.1); */
}
.fixP {
  transform: translate(-0px, -0px);
}
</style>

