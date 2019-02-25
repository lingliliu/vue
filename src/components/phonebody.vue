// 就是中间的列表，因为较多，所以封装成组件
<template>
  <div class="phone-body">
    <!-- 一条动态就是一个vuegram-post，一个动态也包含三部分，id，图片，likes -->
    <div class="feed" v-if="step === 1" v-dragscroll>
      <app-VueGramPost v-for="(post,index) in posts" :key="index" :post="post"></app-VueGramPost>
    </div>
    <div v-if="step === 2">
      <div class="selected-image" :style="{backgroundImage:'url('+ image +')'}" :class="filter"></div>
      <div class="filter-container" v-dragscroll>
        <app-VFilterType v-for="(filter,index) in filters" 
        :key="index" :filter=filter :image='image' @selectfilter='selectedfilter'
        ></app-VFilterType>
      </div>
    </div>
    <div v-if="step === 3">
        <div class="selected-image" :style="{backgroundImage:'url('+ image +')'}" :class="filter"></div>
        <div class="caption-container">
            <textarea placeholder="Write a caption" :value="value" @input="inputtext"></textarea>
        </div>
    </div>
  </div>
</template>
<script>
import VueGramPost from "./VuegramPost.vue";
import FilterType from "./filtertype.vue";
export default {
  // 接收来自父组件的动态数据posts
  props: {
    posts: Array,
    step: Number,
    image: String,
    filters: Array,
    // 这是v-model
    value: String
  },
  data(){
      return {
          filter:''
      }
  },

  components: {
    "app-VueGramPost": VueGramPost,
    "app-VFilterType": FilterType
  },
  methods:{
      selectedfilter(eve){
          this.filter = eve
      },
      inputtext(event){
          this.$emit('input',event.target.value)
      }
  }
};
</script>
// 用scss预处理器
<style lang="scss" src='../styles/phone-body.scss'>
</style>


