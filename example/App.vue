<template>
  <div>
    <uploader
      :options="options"
      :file-status-text="statusText"
      class="uploader-example"
      ref="uploader"
      @file-complete="fileComplete"
      @complete="complete"
    ></uploader>
  </div>
</template>

<script>

const axios = require('axios')

const getQinniuOssparams = async function (file) {
  let token = await axios.get('http://localhost:3000/qiniuuptoken?fileName=' + file.name)
  token = token.data
  console.log('token', token)
  return {
    token,
    key: file.name
  }
}

const getAliOssparams = async function (file) {
  let data = await axios.get('http://localhost:3000/aliuptoken?fileName=' + file.name)
  let ossParams = {
    ossConfig: data.data,
    name: file.name
  }
  return ossParams
}

export default {
  data () {
    return {
      options: {
        target: '//localhost:3000/upload', // '//jsonplaceholder.typicode.com/posts/',
        testChunks: false,
        oss: 'qiniu',
        ossParams: getQinniuOssparams
      },
      attrs: {
        accept: 'image/*'
      },
      statusText: {
        success: '成功了',
        error: '出错了',
        uploading: '上传中',
        paused: '暂停中',
        waiting: '等待中'
      }
    }
  },
  methods: {
    complete () {
      console.log('complete', arguments)
    },
    fileComplete () {
      console.log('file complete', arguments)
    }
  },
  mounted () {
    this.$nextTick(() => {
      window.uploader = this.$refs.uploader.uploader
    })
  }
}
</script>

<style>
.uploader-example {
  width: 880px;
  padding: 15px;
  margin: 40px auto 0;
  font-size: 12px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.4);
}
.uploader-example .uploader-btn {
  margin-right: 4px;
}
.uploader-example .uploader-list {
  max-height: 440px;
  overflow: auto;
  overflow-x: hidden;
  overflow-y: auto;
}
</style>
