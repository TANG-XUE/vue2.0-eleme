<template>
  <div class="ratingselect">
    <div class="rating-type border-1px">
      <span class="block positive" :class="{'active':sType===2}" @click="select(2,$event)">{{desc.all}}<span class="count">{{ratings.length}}47</span></span>
      <span class="block positive" :class="{'active':sType===0}" @click="select(0,$event)">{{desc.positive}}<span class="count">{{positives.length}}</span></span>
      <span class="block negative" :class="{'active':sType===1}" @click="select(1,$event)">{{desc.negative}}<span class="count">{{negatives.length}}</span></span>
    </div>
    <div @click="toggleContent" class="switch" :class="{'on':oContent}">
      <span class="icon-check_circle" ></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
  const POSITIVE=0;//满意的评价
  const NEGATIVE=1;//不满意评价
  const ALL=2; //所有的评价

    export default {
      name: "RatingSelect",
      props:{
        ratings:{
          type: Array,
          default() {
            return [];
          }
        },
        selectType: {
          type: Number,
          default: ALL
        },
        onlyContent: {
          type: Boolean,
          default: false
        },
        desc:{
          type:Object,
          default() {
            return {
              all: '全部',
              positive: '满意',
              negative: '不满意'
            };
          }
        }
      },
      data() {
          return {
            sType:this.selectType,
            oContent:this.onlyContent
          }
      },
      computed:{
        positives(){
          return this.ratings.filter((rating)=>{
            return rating.rateType===POSITIVE;
          })
        },
        negatives(){
          return this.ratings.filter((rating)=>{
            return rating.rateType===NEGATIVE;
          })
        }
      },
      methods:{
        select(type,event){
          if(!event._constructed){
            return;//浏览器直接return掉,去掉自带click事件的点击
          }
          //将this.selectType设置成传入的参数,而不是food传过来的初始化的值，之后样式就可以随着点击改变了
          this.sType=type;
          //派发事件通知父组件food.vue selectType的改变,将type值传出去
          this.$emit("increment","selectType",type)
        },
        toggleContent(event){
          if(!event._constructed){
            return;
          }
          this.oContent=!this.oContent;
          this.$emit("increment","onlyContent",this.oContent)
        }
      },
      components: {}
    }

</script>

<style lang="stylus" scoped>
  @import "../../assets/style/stylus/mixin.styl";
  .ratingselect
    .rating-type
      padding: 18px 0
      margin: 0 18px
      border-1px(rgba(7, 17, 27, 0.1))
      font-size: 0
      .block
        display: inline-block
        padding: 8px 12px
        margin-right: 8px
        line-height: 16px
        border-radius: 1px
        font-size: 12px
        color: rgb(77, 85, 93)
        &.active
          color: #fff
        .count
          margin-left: 2px
          font-size: 8px
        &.positive
          background: rgba(0, 160, 220, 0.2)
          &.active
            background: rgb(0, 160, 220)
        &.negative
          background: rgba(77, 85, 93, 0.2)
          &.active
            background: rgb(77, 85, 93)
    .switch
      padding: 12px 18px
      line-height: 24px
      border-bottom: 1px solid rgba(7, 17, 27, 0.1)
      color: rgb(147, 153, 159)
      font-size: 0
      &.on
        .icon-check_circle
          color: #00c850
      .icon-check_circle
        display: inline-block
        vertical-align: top
        margin-right: 4px
        font-size: 24px
      .text
        display: inline-block
        vertical-align: top
        font-size: 12px
</style>
