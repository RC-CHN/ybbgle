<template>
  <div class="container" @click="navigate">
    <div class="fixPos">
      <img :src="iconUrl" class="icon" />
    </div>
    <div class="title fixPos">{{ title }}</div>
  </div>
  <div class="closeIcon">
    <img src="./close.svg" @click="deleteSite" />
  </div>
  
</template>

<script>
import { Close } from "@element-plus/icons-vue";
import { ElMessageBox } from "element-plus";
export default {
  props: ["content"],
  name: "urlItem",
  components: {
    ElMessageBox,
  },
  data() {
    return {
      dialogVisible: false,
      iconUrl: null,
      url: null,
      title: null,
    };
  },
  mounted() {
    console.log("挂载内容");
    console.log(this.$props.content);
    if (this.$props.content.icon == undefined) {
      this.$data.iconUrl = this.$props.content.url + "/favicon.ico";
    } else {
      this.$data.iconUrl = this.$props.content.icon;
    }
    this.$data.title = this.$props.content.title;
    this.$data.url = this.$props.content.url;
    console.log(this.$data);
  },
  methods: {
    navigate() {
      console.log("navigate");
      window.open(this.$data.url);
    },
    getUrl() {},
    deleteSite() {
      console.log("删除站点");
	  this.$emit("delete",this.$data.title)
    },
  },
};
</script>

<style>
.container {
  width: 100px;
  height: 100px;
  /* max-height: 40px; */
  border: 1px solid transparent;
  text-decoration: none;
  color: #515151;
  background-color: #ffffff;
  opacity: 0.8;
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
  border-radius: 20px;
  margin-top: 10px;
  box-shadow: 2px 5px 5px 5px rgba(0, 0, 0, 0.1);
  margin-left: 10px;
  margin-right: 10px;
}

.title {
  font-size: 10px;
  white-space: nowrap;
}

.container:hover {
  background-color: #c0c0c0;
}

.icon {
  height: 50px;
  width: 50px;
  transform: translate(0px, 0px);
}

.fixPos {
  /* position: relative;
		top: 10rpx;
		margin-left: 10rpx;
		margin-right: 10rpx;
		transform: translateY(-10px); */
}
.closeIcon {
  width: 15px;
  height: 15px;
  position: relative;
  top: 5px;
  left: -15px;
  opacity: 0%;
}
.closeIcon:hover {
  opacity: 20%;
}
.closeIcon:active {
  opacity: 50%;
}
</style>
