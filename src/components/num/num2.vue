<template>
 <div class="num2">
    <h3 class="orderTitle">订单总量</h3>
    <div class="box-item">
     <li :class="{'number-item': !isNaN(item), 'mark-item': isNaN(item) }"
      v-for="(item,index) in orderNum" :key="index">
       <span v-if="!isNaN(item)">
        <i ref="numberItem">0123456789</i>
       </span>
      <span class="comma" v-else>{{item}}</span>
     </li>
    </div>
   </div>
</template>
<script>
export default {
  data () {
    return {
      num: 100,
      orderNum: ['0', '0', '0', ',', '0', '0', '0'] // 默认订单总数
    }
  },
  mounted () {
    this.toOrderNum(this.num)
    setInterval(() => {
      this.num++
      this.toOrderNum(this.num)
    }, 1000)
  },
  methods: {
    // 设置文字滚动
    setNumberTransform () {
      const numberItems = this.$refs.numberItem // 拿到数字的ref，计算元素数量
      const numberArr = this.orderNum.filter(item => !isNaN(item))
      // 结合CSS 对数字字符进行滚动,显示订单数量
      for (let index = 0; index < numberItems.length; index++) {
        const elem = numberItems[index]
        elem.style.transform = `translate(-50%, -${numberArr[index] * 10}%)`
      }
    },
    // 处理总订单数字
    toOrderNum (num) {
      num = num.toString()
      // 把订单数变成字符串
      if (num.length < 6) {
        num = '0' + num // 如未满八位数，添加"0"补位
        this.toOrderNum(num) // 递归添加"0"补位
      } else if (num.length === 6) {
        // 订单数中加入逗号
        num = num.slice(0, 3) + ',' + num.slice(3, 6)
        this.orderNum = num.split('') // 将其便变成数据，渲染至滚动数组
      } else {
        // 订单总量数字超过八位显示异常
        this.$message.warning('订单总量数字过大，显示异常，请联系客服')
      }
      this.setNumberTransform()
    }
  }
}
</script>
<style scoped lang='scss'>
  /*订单总量滚动数字设置*/
 .box-item {
  position: relative;
  height: 100px;
  font-size: 54px;
  line-height: 41px;
  text-align: center;
  list-style: none;
  color: #2D7CFF;
  writing-mode: vertical-lr;
  text-orientation: upright;
  user-select: none;
 }
 /* 默认逗号设置 */
 .mark-item {
  width: 10px;
  height: 100px;
  margin-right: 5px;
  line-height: 10px;
  font-size: 48px;
  position: relative;
  & > span {
   position: absolute;
   width: 100%;
   bottom: 0;
   writing-mode: vertical-rl;
   text-orientation: upright;
  }
 }
 /*滚动数字设置*/
 .number-item {
  width: 41px;
  height: 75px;
  background: #ccc;
  list-style: none;
  margin-right: 5px;
  background:rgba(250,250,250,1);
  border-radius:4px;
  border:1px solid rgba(221,221,221,1);
  & > span {
   position: relative;
   display: inline-block;
   margin-right: 10px;
   width: 100%;
   height: 100%;
   writing-mode: vertical-rl;
   text-orientation: upright;
   overflow: hidden;
   & > i {
    font-style: normal;
    position: absolute;
    top: 11px;
    left: 50%;
    transform: translate(-50%,0);
    transition: transform 1s ease-in-out;
    letter-spacing: 10px;
   }
  }
 }
 .number-item:last-child {
  margin-right: 0;
 }
</style>
