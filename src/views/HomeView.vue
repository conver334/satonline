<template>
  <div class="common-layout">
    <el-container>
      <el-aside width="40%">
        <el-button type="primary" @click="buttonchose=true"  round> 文字输入 </el-button>
        <el-button type="primary" @click="buttonchose=false" round> 文档上传 </el-button>
        <div v-if="buttonchose">
          子句个数<el-input-number v-model="clausenum" :min="1" :max="10" @change="handleChange" />
          变量个数<el-input-number v-model="valnum" :min="1" :max="10" @change="handleChange" />
          <el-input
            v-model="textarea"
            :autosize="{ minRows: 2, maxRows: 10000000}"
            type="textarea"
            placeholder="input cnf"
          />
        </div>
        <div v-else>
         
        </div>
      </el-aside>
      <el-main>Main</el-main>
    </el-container>
  </div>

</template>

<script >
import { ref } from 'vue'
export default{
setup(){
  const buttonchose = ref(true)
  const clausenum = ref(1)
  const valnum = ref(1)
  const handleChange = ()=>{
    console.log("change")
  }
  return {
    buttonchose,clausenum,valnum,handleChange
  }
}
}


</script>