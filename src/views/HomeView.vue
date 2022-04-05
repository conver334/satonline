<template>
 
    <el-row>
      <el-col :span="12">
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
           <el-upload
              class="upload-demo"
              action="https://jsonplaceholder.typicode.com/posts/"
              :on-preview="handlePreview"
              :on-remove="handleRemove"
              :before-remove="beforeRemove"
              multiple
              :limit="3"
              :on-exceed="handleExceed"
              :file-list="fileList">
              <el-button size="small" type="primary">点击上传</el-button>
              <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
            </el-upload>
        </div>
      </el-col>
      <el-col :span="12">
        <el-row > 
          <el-col :span="8"><div class="grid-content bg-purple">
            ceshi
            </div></el-col>
          <el-col :span="8"><div class="grid-content bg-purple-light">
            ceshi
            </div></el-col>
          <el-col :span="8"><div class="grid-content bg-purple-light">
            
            </div></el-col>
        </el-row>

      </el-col>
    </el-row>


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
  },
  data() {
    return {
      fileList: [{name: 'food.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}, {name: 'food2.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}]
    };
  },
  methods: {
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePreview(file) {
      console.log(file);
    },
    handleExceed(files, fileList) {
      this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
    },
    beforeRemove(file, fileList) {
      return this.$confirm(`确定移除 ${ file.name }？`);
    }
  }
}


</script>