// 一个vuegram-post就是一条动态，想着根据数据动态加载，所以把一个动态独立成组件,将所有post[0]换成post
<template>
  <div class="vuegram-post">
    <div class="header level">
      <!-- 用bulma库里的布局，level -->
      <div class="level-left">
        <!-- 头像 -->
        <figure class="image is-32x32">
          <!-- 按说动态从后台加载posts ，记得要v-bind-->
          <img :src="post.userImage">
        </figure>
        <span class="username">{{ post.username}}</span>
      </div>
    </div>
    <!-- 和header不一样，这里直接给成背景图片，也要绑定style，不能忘了{} -->
    <!-- 滤镜用的是fliters属性 -->
    <!-- 双击图片能点赞或取消赞，绑定事件 -->
    <div
      class="image-container"
      :style="{backgroundImage:'url('+ post.postImage +')'}"
      :class="post.filter"
      @dblclick='clicklike'
    ></div>
    <div class="content">
      <div class="heart">
          <!-- 单击心点赞或取消赞，绑定事件 -->
        <!-- fas是实心，far是空心，来改变空心或实心，改变赞 -->
        <i class="far fa-heart" 
        @click="clicklike"
        :class="{fas:this.post.hasBeenLiked}"></i>
      </div>
      <p class="likes">{{ post.likes }} likes</p>
      <p class="caption">
        <span>{{ post.username}}</span>
        {{ post.caption}}
      </p>
    </div>
  </div>
</template>
<script>
export default {
    props:{
        post:Object
    },
    methods:{
        clicklike(){
            // 打印一下this.post,可以看出里面有个字段能够判断当前图标状态hasBeenLiked是true false
            // console.log(this.post)
            // 如果当前状态是true那么likes数量要减一
            this.post.hasBeenLiked ? this.post.likes-- : this.post.likes++
            // 要取反，否则一直加
            this.post.hasBeenLiked = !this.post.hasBeenLiked
        }
    }
};
</script>
<style lang="scss" src='../styles/vuegram-post.scss'>
</style>

