// 滤镜列表的
<template>
  <div class="filter-type">
    <p>{{ filter.name }}</p>
    <div class="img" :style="{backgroundImage:'url('+ image +')'}" 
    :class="filter.name" @click="selectfilter"></div>
  </div>
</template>

// 在子组件filtertype点击，改变父组件phonetype

<script>
export default {
    props:{
        // 注意这里不是数组，而是对象
        filter:Object,
        // 注意image也要传
        image:String
    },
    methods:{
        selectfilter(){
            // 子组件向父组件传，通过自定义事件
            this.$emit("selectfilter",this.filter.name)
        }
    }
};
</script>
<style lang="scss" src='../styles/filter-type.scss'>
</style>