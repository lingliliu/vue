<template>
  <div id="app">
    <!-- 先用app-phone包裹三部分，header，列表，尾部 -->
    <div class="app-phone">
      <div class="phone-header">
        <a class="cancel-cta" v-if="step===2 || step===3" @click="goHome">Cancel</a>
        <!-- 头部有张图片 -->
        <img src="./assets/vue_gram_logo_cp.png">
        <a class="next-cta" v-if="step===2" @click="step++">Next</a>
        <a class="next-cta" v-if="step===3" @click="share">Share</a>
      </div>

      <app-phonebody
        :posts="Postsdata"
        :step="step"
        :image="image"
        :filters="filters"
        v-model="caption"
      ></app-phonebody>
      <div class="phone-footer">
        <div class="home-cta">
          <!-- 引入图标库 -->
          <i class="fas fa-home fa-lg" @click="goHome"></i>
        </div>
        <!-- 点击文件上传，用input -->
        <div class="upload-cta">
          <!-- 在样式里设置name等于file就隐藏 -->
          <!-- 上传文件就会触发change事件 -->
          <input
            type="file"
            id="file"
            name="file"
            @change="uploadImg"
            accept="image/jpg, image/png, image/jpeg, image/gif"
            :disabled="step !== 1"
          >
          <!-- 把图标放进label，点击图标就会相当于点击input框 -->
          <label for="file">
            <i class="far fa-plus-square fa-lg"></i>
          </label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import phoneBody from "./components/phonebody.vue";

// 按说应该后台给，这里模拟后台返回的数据，用js文件代替
import Postsdata from "./data/posts.js";

// 引入滤镜类名
import filters from "./data/filters.js";
export default {
  name: "app",
  data() {
    return {
      Postsdata,
      step: 1,
      image: "",
      filters,
      caption: ""
    };
  },
  components: {
    "app-phonebody": phoneBody
  },
  methods: {
    uploadImg(event) {
      // 用变量files将上传的文件存一下
      const files = event.target.files;
      // !files.length相当于files.length==0，没上传
      // console.log(event)
      if (!files.length) {
        return;
      }

      // 添加图片并上传
      const reader = new FileReader();
      reader.readAsDataURL(files[0]);
      reader.onload = e => {
        // console.log(e)
        this.image = e.target.result;
        // 上传成功后，会跳到滤镜那个页面，所以phonebody变了，想到用v-if进行dom节点替换
        this.step = 2;
      };

      // 引用库，添加滤镜
    },
    goHome() {
      (this.step = 1), (this.image = "");
    },
    share() {
      const post = {
        username: "MOSS",
        userImage:
          "https://s3-us-west-2.amazonaws.com/s.cdpn.io/1211695/me_3.jpg",
        postImage:this.image,
        likes: 0,
        hasBeenLiked: false,
        caption: this.caption,
        filter: this.filter
      }
      this.Postsdata.unshift(post)
      this.goHome()
    }
  }
};
</script>

<style lang="scss" src='./styles/app.scss'>
</style>
