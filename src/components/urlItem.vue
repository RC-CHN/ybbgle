<template>
  <div class="container" @click="navigate">
    <div class="fixPos">
      <image :src="iconUrl" class="icon"></image>
    </div>
    <div class="title fixPos">{{ title }}</div>
  </div>
</template>

<script>
export default {
  props: ["content"],
  name: "urlItem",
  data() {
    return {
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
  },
};
</script>

<style>
.container {
		height: 50rpx;
		border: 1px solid transparent;
		text-decoration: none;
		color: rgb(255, 255, 255);
		display: flex;
		flex-direction: row;
		justify-content: flex-start;
		border-radius: 20rpx;
		margin-top: 20rpx;
		/* margin-left: 50rpx;
		margin-right: 50rpx; */
	}

	.container:hover {
		background-color: rgb(91, 91, 99);
	}

	.icon {
		height: 20px;
		width: 20px;
	}

	.fixPos {
		position: relative;
		top: 10rpx;
		margin-left: 10rpx;
		margin-right: 10rpx;
	}
</style>
