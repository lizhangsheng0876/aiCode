<script setup>
// 获取环境变量
import { onBeforeMount } from '@vue/runtime-core'
import { Configuration, OpenAIApi } from 'openai'
const ENV = import.meta.env
const configuration = new Configuration({
  organization: 'org-7S7l13xcdiwTCaz3UG3fnVyN',
  apiKey: ENV.VITE_APP_OPENAI_API_KEY
})
const openai = new OpenAIApi(configuration)
console.log(openai)
onBeforeMount(async () =>{
    console.log("onBeforeMount")
})

let areaText = ref(null)
// let isShow = ref(false)
let searchText = ref(null)
async function handleSubmit(){
  // isShow.value = !isShow.value;
  console.log(searchText.value,"searchText")
  let res = await openai.createCompletion({
    "model": "text-davinci-003",
    "prompt": searchText.value,
    "max_tokens": 256,
    "temperature": 0,
    "top_p": 1,
    "n": 1,
  });
  areaText.value = res.data.choices[0].text
}
  
</script>
<template>
  <!-- <a-spin v-if="isShow" /> -->
  <a-space size="medium">
  <div style="width: 600px;margin-top: 20px;">
    <a-textarea placeholder="请输入内容" style="height:600px" v-model="searchText"></a-textarea>
    <div style="text-align: center;margin-top: 10px;">
      <a-button type="primary" @click="handleSubmit">提交</a-button>
    </div>
  </div>
  <a-divider direction="vertical" />
  <div style="width: 600px;margin-top: 20px;">
    <a-textarea style="height:600px" v-model="areaText"></a-textarea>
    <div style="text-align: center;margin-top: 10px;">
      <a-button type="primary">清空</a-button>
    </div>
  </div>
  </a-space>
</template>
<style>
</style>
