<template>
 <div class="enviroment">
    <p class="heal-index"> 平均健康指数</p>
    <div class="healthy-index">

      <span>差</span>&nbsp;&nbsp;

      <div class="healthy-mark">

        <p class="inner" :style="`border-top-color: ${pointer(innerValue).color};transform: translateX(${pointer(innerValue).left})`">
          <span>室内</span>
        </p>

        <div class="mid">
          <ul class="rectangle">
            <li v-for="item in colors" :key="item" :style="`width:${width};background-color:${item}`"></li>
          </ul>
        </div>

        <p class="outer" :style="`border-bottom-color: ${pointer(outerValue).color};transform: translateX(${pointer(outerValue).left})`">
          <span>室外</span>
        </p>

      </div>

      &nbsp;<span>优</span>
    </div>
    <p class="outside-tp tips">室外环境</p>
    <div class="outside-tp temp"><span>温度：25℃</span><span >CO2:420ppm</span></div>
    <div class="outside-tp"><span>湿度：50%</span><span>PM2.5：74</span></div>
 </div>
</template>

<script>
export default {
  components: {
  },
  mixins: [],
  props: {
    width: {
      type: String,
      default: '66px'
    },
    colors: {
      type: Array,
      default: function () {
        return ['#f58788', '#f8b95c', '#77d99b', '#4fa0f6']
      }
    },
    innerValue: {
      type: Number,
      default: 40
    },
    outerValue: {
      type: Number,
      default: 50
    }
  },
  data () {
    return {
      minValue: 0,
      maxValue: 100
    }
  },
  computed: {
  },
  mounted () {
    console.log(this.index(this.innerValue))
  },
  methods: {
    // 返回指针对象
    pointer (currentValue) {
      const itemWidth = parseFloat(this.width)
      // 占比
      const per = (currentValue - this.minValue) / (this.maxValue - this.minValue)
      const left = per * (itemWidth * this.colors.length)
      const index = this.index(currentValue)
      return {
        left: `${left + 4 * index}px`,
        color: this.colors[index]
      }
    },
    // 位于第几个
    index (currentValue) {
      let index = Math.ceil(parseFloat(currentValue) / 100 * this.colors.length) - 1
      index = index === -1 ? 0 : index
      return index
    }
  }
}
</script>

<style scoped lang="less" >
 .heal-index {
      padding-left: 20px;
      padding-top: 20px;
      color: #3b4254;
    }
    .outside-tp {
      margin-left: 20px;
      color: #62697B;
      span:first-child {
          display: inline-block;
          width: 137px;
      }
    }
    .outside-tp.tips {
        color: #3B4254;
    }
    .outside-tp.temp {
        padding: 16px 0 10px 0;
    }
  .healthy-index {
    display: flex;
    align-items: center;
    padding: 43px 0;
    padding-left: 30px;
    .healthy-mark {
      position: relative;
      .inner, .outer {
        position: absolute;
        left: -3px; // 抵消三角形的边距
        top: -8px;
        width: 0;
        height: 0;
        border-left:3px solid transparent;
        border-right:3px solid transparent;
        border-top:5px solid; /*--三角形的高--*/
        span {
          position: absolute;
          display: inline-block;
          top: -24px;
          left: -12px;
          width: 100px;
        }
      }
      .outer {
        top: 15px;
        border-top: none;
        border-bottom: 5px solid; /*--三角形的高--*/
        span {
          position: absolute;
          display: inline-block;
          top: 6px;
          left: -12px;
          width: 100px;
        }
      }
      .mid {
        ul.rectangle {
          display: flex;
          justify-content: space-between;
          align-items: center;
          li {
            margin-right: 4px;
            height: 12px;
            border-radius: 1px;
          }
        }
      }
    }
  }
</style>