<template>
  <div>
    <ul>
      <transition-group @after-leave="afterLeave">
        <img
          :src="item.imageUrl"
          alt=""
          v-for="(item, index) in banner"
          :key="index"
          :style="{'opacity':(showIndex==index?'1':'0')}"
          @click="right"
        />
      </transition-group>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
      banner: [],
      showIndex: 0,
      isEnd: true,
      intervalID:null
    };
  },
  mounted() {
     //轮播图
    this.$nextTick(()=>{
      this.$axios.get("/banner").then((d) => {
      this.banner = d.data.banners.splice(0, 4);
    });
    })
    //加载组件并渲染完成
    this.autostart();
  },
  beforeDestroy() {
    this.stop(); //销毁定时器..
  },
  methods: {
    stop() {
      clearInterval(this.intervalID);
    },
    autostart() {
      this.intervalID = setInterval(() => {
        this.right();
      }, 3000);
    },
    // left() {
    //   if (!this.isEnd) return;
    //   if (this.showIndex <= 0) {
    //     this.showIndex = this.banner.length;
    //   }
    //   this.showIndex--;
    //   this.isEnd = false;
    // },
    right() {
      if (!this.isEnd) return;
      this.showIndex++;
      if (this.showIndex >= this.banner.length) {
        this.showIndex = 0;
        this.isEnd = false;
      }
    },
    afterLeave() {
      this.isEnd = true;
    },
  },
};
</script>
<style lang="less" scoped>
div {
  ul {
    position: relative;
    width: 100vw;
    height: 40vw;
    overflow: hidden;
    }
    img {
      position: absolute;
      top: 0;
      right: 0;
      width: 100%;
      height: 100%;
      opacity: 0;
      transition: all 1s 0s linear;
    }
  }
</style>