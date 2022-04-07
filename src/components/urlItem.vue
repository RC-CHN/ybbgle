<template>
  <div class="container" @click="navigate">
    <div class="fixPos">
      <img :src="iconUrl" class="icon">
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
		height: 50px;
		border: 1px solid transparent;
		text-decoration: none;
		color: rgb(255, 255, 255);
		display: flex;
		flex-direction: row;
		justify-content: flex-start;
		border-radius: 20px;
		margin-top: 20px;
		/* margin-left: 50px;
		margin-right: 50px; */
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
		top: 10px;
		margin-left: 10px;
		margin-right: 10px;
	}
</style>
