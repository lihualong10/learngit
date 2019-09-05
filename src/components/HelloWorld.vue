<template>
  <div class="hello">
    <van-uploader accept="video" :preview-image="false" v-model="fileList" :after-read="afterRead"/>
    <ul>
      <li v-for='(url,index) in imgList' :key="index">
        <i class='del iconfont icon-close' @click.stop='delImg(index)'></i>
        <video  :src="url" controls></video>
       </li>
    </ul>
<!--    accept="video"-->
    <div class="app">
      <div ref="msgDiv">{{msg}}</div>
      <div v-if="msg1">Message got outside $nextTick: {{msg1}}</div>
      <div v-if="msg2">Message got inside $nextTick: {{msg2}}</div>
      <div v-if="msg3">Message got outside $nextTick: {{msg3}}</div>
      <button @click="changeMsg">
        Change the Message
      </button>
    </div>
    <button @click="$router.push(`one`)">goNext</button>
  </div>
</template>
<script>
import upLoaderImg from '../../static/js/Upload'
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Hello Vue.',
      msg1: '',
      msg2: '',
      msg3: '',
      fileList: [],
      imgList: []
    }
  },
  beforeRouteEnter (from, to, next) {
    next(vm => {
      vm.msg = 'Hello Vm'
    })
  },
  beforeCreate () {
    // alert('beforeCreate')
  },
  created () {
    // alert('created')
    this.resetmt(1, 3, 9)
  },
  beforeMount () {
    // alert('beforeMount')
  },
  mounted () {
    // alert('mounted')
  },
  beforeUpdate () {
    // alert('beforeUpdate')
  },
  updated () {
    // alert('updated')
  },
  beforeDestroy () {
    // alert('beforeDestroy')
  },
  destroyed () {
    // alert('destroyed')
  },
  methods: {
    resetmt (...val) {
      console.log(val.length)
      let arr = []
      for (let k of val) {
        arr.push(k)
      }
      console.log(arr)
    },
    changeMsg () {
      this.msg = 'Hello world.'
      this.msg1 = this.$refs.msgDiv.innerHTML
      this.$nextTick(() => {
        this.msg2 = this.$refs.msgDiv.innerHTML
      })
      this.msg3 = this.$refs.msgDiv.innerHTML
    },
    async afterRead (file) { // 文件读取完成后的回调函数
      console.log(this.fileList)
      let uploadImg = await upLoaderImg(file.file) // 使用上传的方法。file.file
      // console.log(uploadImg.map.fileAliUrl)
      this.imgList.push(uploadImg.map.fileAliUrl)
      console.log(this.imgList)
    },
    delImg (index) {
      this.msg = '已删除'
      // this.imgArr.splice(index, 1)
      // this.$emit('showImg2', this.imgArr)
      // this.size = this.size - this.imgList[index].file.size // 总大小
      this.imgList.splice(index, 1)
      // if (this.limit !== undefined) this.limit = 5 - this.imgList.length
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
p{
  background-color: aqua;
}
li{
  position: relative;
}
video{
  width: 100px;
  height: 100px;
}
.del {
  position: absolute;
  width: 18px;
  height: 10px;
  top: 0;
  background: #f00;
  right: 0;
  z-index: 999
}
</style>
