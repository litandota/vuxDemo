<template>
  <div>
    <x-header>已办事项</x-header>



    <flexbox>
      <flexbox-item :span="9">
        <search
          @result-click="resultClick"
          @on-change="getResult"
          :results="results"
          v-model="value"
          position="absolute"
          auto-scroll-to-top
          top="46px"
          @on-focus="onFocus"
          @on-cancel="onCancel"
          @on-submit="onSubmit"
          ref="search"></search>
      </flexbox-item>
      <flexbox-item :span="3">
        <x-button @click.native="moreSearch()" style="font-size: 13px;height: 45px">高级  <x-icon type="ios-arrow-forward" size="12"></x-icon></x-button>
      </flexbox-item>
    </flexbox>



    <div class="moreSearch" v-if="isShow">
      <x-input title="经办人"  placeholder="输入经办人"></x-input>

      <x-input title="当前审批节点"  placeholder="输入审批节点"></x-input>

      <datetime-range title="开始时间" start-date="2017-01-01" end-date="2017-02-02"  v-model="startTimeValue" ></datetime-range>

      <datetime-range title="结束时间" start-date="2017-01-01" end-date="2017-02-02"  v-model="endTimeValue" ></datetime-range>

    </div>




    <scroller :style="scrollerStyle"
              :on-refresh="refresh"
              :on-infinite="infinite">
      <div v-for="(item, index) in items" class="row" :class="{'grey-bg': index % 2 == 0}">
        {{ item }}
      </div>
    </scroller>









  </div>


</template>

<script>

  import { XHeader,Search,XButton,Cell,Flexbox, FlexboxItem ,XInput  ,DatetimeRange  } from 'vux'


export default {
  components: {
    XHeader,Search,XButton,Cell,Flexbox, FlexboxItem,XInput,DatetimeRange
  },
  data () {
    return {
      msg: 'Hello World!',
      results: [],
      value: '',
      startTimeValue: [],
      endTimeValue: [],
      isShow:false,
      items: [],
      scrollerStyle:'top:110px'

    }
  },
  mounted() {
    for (var i = 1; i <= 20; i++) {
      this.items.push(i + ' - keep walking, be 2 with you.')
    }
    this.top = 1
    this.bottom = 20
  },
  methods: {

    refresh (done) {
      setTimeout(() => {
        var start = this.top - 1
        for (var i = start; i > start - 10; i--) {
          this.items.splice(0, 0, i + ' - keep walking, be 2 with you.')
        }
        this.top = this.top - 10
        done()
      }, 1500)
    },
    infinite (done) {
      setTimeout(() => {
        var start = this.bottom + 1
        for (var i = start; i < start + 10; i++) {
          this.items.push(i + ' - keep walking, be 2 with you.')
        }
        this.bottom = this.bottom + 10
        done()
      }, 1500)
    },
    moreSearch(){
      if(!this.isShow){
        this.isShow = true;
          this.scrollerStyle = 'top:290px'
      }else {
        this.isShow = false;
        this.scrollerStyle = 'top:110px'
      }
    },
    resultClick (item) {
      window.alert('you click the result item: ' + JSON.stringify(item))
    },
    getResult (val) {
      console.log('on-change', val)
      this.results = val ? getResult(this.value) : []
    },
    onSubmit () {
      console.log('搜索');
    },
    onFocus () {
      console.log('on focus')
    },
    onCancel () {
      console.log('on cancel')
    }
  },
}
</script>

<style>
  html, body {
    margin: 0;
  }
  * {
    box-sizing: border-box;
  }
  .row {
    width: 100%;
    height: 50px;
    padding: 10px 0;
    font-size: 16px;
    line-height: 30px;
    text-align: center;
    color: #444;
    background-color: #fff;
  }
  .grey-bg {
    background-color: #eee;
  }
  .header {
    position: fixed;
    top: 0;
    left: 0;
    height: 44px;
    width: 100%;
    box-shadow: 0 2px 10px 0 rgba(0,0,0,0.1);
    background-color: #fff;
    z-index: 1000;
    color: #666;
  }
  .header > .title {
    font-size: 16px;
    line-height: 44px;
    text-align: center;
    margin: 0 auto;
  }
</style>
