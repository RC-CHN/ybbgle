
<template>
  <div class="content" v-if="reloadFlag">
    <div>
      <img
        src="../../static/logo.png"
        class="logo"
        v-if="status"
        @click="changeStatus"
      >
      <div v-if="!status">
        <div>
          <img
            src="../../static/long.png"
            class="long"
            @click="changeStatus()"
          >
        </div>
        <div> 喔唷，崩溃啦！ </div>
        <div @click="toUpload"> 显示 你妈 时出了些问题。 </div>
        <div style="font-weight: 700"> 可能是你妈不存在 </div>
        <div style="color: #007aff" @click="navigate"> 了解详情 </div>
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
      
      <urlItem v-for="content in sites" v-bind:content="content" v-bind:key="content.url"></urlItem>
      <!-- <div class="nav-panel-item" title="Bilibili" href="https://www.bilibili.com/" target="_blank">
			        <img src="https://www.bilibili.com/favicon.ico" alt="Bilibili" height="16" width="16" class="nav-panel-item-img">
			        <div class="nav-panel-item-txt">Bilibili</div>
			    </div> -->
      <!-- <addPage></addPage> -->
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import urlItem from "@/components/urlItem.vue";
// import * as echarts from "echarts"
import axios from "axios";
export default {
  name: "HomeView",
  components: {
    urlItem
  },
  data() {
    return {
      title: "Hello",
      searchText: "",
      sites: null,
      status: true,
      reloadFlag: false,
    };
  },
  mounted() {
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
    changeStatus() {
      this.$data.status = !this.$data.status;
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
		margin-top: 70px;
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
		font-family: 'Microsoft YaHei UI', arial, sans-serif;
		font-size: 15px;
		color: #555;
		border: none;
		background: #fff;
		overflow: hidden;
		outline: none;
		vertical-align: middle;
		border-radius: 2px;
		background-color: #FFFFFF;
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
		margin-top: 20rpx;
		/* margin-bottom: 100px; */
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
		/* box-shadow: 2px 5px 5px 5px rgba(0, 0, 0, 0.1); */
	}
</style>

