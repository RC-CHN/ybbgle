<template>
  <div class="container" @click="addPage">
    <div class="fixPos">
      <img :src="iconUrl" class="icon" />
    </div>
    <div class="title fixPos">{{ title }}</div>
  </div>
</template>

<script>
export default {
  name: "addSite",
  data() {
    return {
      iconUrl: "http://m.rcfortress.site:7899/static/add.png",
      title: "添加站点",
    };
  },
  mounted() {
    console.log("挂载添加内容");
  },
  methods: {
    addPage() {
      console.log("开始添加站点");
      
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
</style>
