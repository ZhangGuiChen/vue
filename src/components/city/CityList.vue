<template>
    <div class="list" ref="wrapper">
      <div>
        <div class="area">
          <div class="title border-topbottom">当前城市</div>
          <div class="button-wrapper">
            <div class="button">{{ this.$store.state.city}}</div>
          </div>
        </div>
        <div class="area">
          <div class="title border-topbottom" >热门城市</div>
          <div class="button-wrapper" v-for="(item,index) of hotCities" :key="index"  @click="handleClick(item.name)">
            <div class="button">{{ item.name }}</div>
          </div>
        </div>
        <div class="area" v-for="(value,key) of cities" :key="key" :ref="key">
          <div class="title border-topbottom" >{{ key }}</div>
          <div class="item-list" v-for="item of value" :key="item.id" @click="handleClick(item.name)">
            <div class="item border-bottom"
            >{{ item.name }}</div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
import Bscroll from 'better-scroll'
import Bus from '../../assets/bus.js'
export default {
  name: 'CityList',
  data () {
    return {
      letter: ''
    }
  },
  props: {
    cities: Object,
    hotCities: Array
  },
  methods: {
    handleClick (name) {
      this.$store.dispatch('changeCity', name)
      this.$router.push('/')
    }
  },
  mounted () {
    var vm = this
    vm.scroll = new Bscroll(this.$refs.wrapper, {
      click: true
    })
    Bus.$on('scrollEle', function (data) {
      // console.log(this)---Vue
      // console.log(vm)---此组件
      // 这里的this不是vm
      vm.letter = data
      vm.scroll.scrollToElement(vm.$refs[data][0])
    })
  }
}
</script>

<style scoped lang="stylus">
  .list
    position: absolute
    overflow hidden
    top: 1.58rem
    left: 0
    right: 0
    bottom: 0

  .border-topbottom
    &:before
      border-color: #ccc
    &:after
      border-color: #ccc
  .border-bottom
    &:before
      border-color: #ccc
  .area
     background #fff
     overflow hidden
    .title
      line-height: 0.54rem;
      background: #eee;
      padding-left: 0.2rem;
      color: #666;
      font-size: 0.26rem;
    .button-wrapper
      background: white;
      width: 25%;
      padding: 0.15rem;
      float: left
      box-sizing border-box
      .button
        background: white;
        line-height: 0.4rem;
        text-align: center;
        border: 1px solid #ccc;

  .item
    line-height: 0.6rem;
    padding: 0 0.4rem 0 0.18rem;
</style>
