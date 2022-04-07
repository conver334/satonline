<template>
 
    <el-row>
      <el-col :span="12">
        <div class="data_section">
        <el-button type="primary" @click="buttonchose=true"  round class="data_list"> 文字输入 </el-button>
        <el-button type="primary" @click="buttonchose=false" round class="data_list"> 文档上传 </el-button>
        </div>
        <div class="data_section" v-if="buttonchose">
          <block>子句个数<el-input-number v-model="clausenum" :min="1" :max="10" @change="handleChange" /></block> 
          <block>变量个数<el-input-number v-model="valnum" :min="1" :max="10" @change="handleChange" /></block> 
        <div class="data_section" >
          <el-input 
            v-model="cnfformula"
            :autosize="{ minRows: 2, maxRows: 10000000}"
            type="textarea"
            placeholder="input cnf"
            @blur="checkvalid"
          />
        </div>
          <div v-if="error">{{error}}</div>
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
        <el-row class="data_section" > 
          <el-col :span="8"><div class="grid-content bg-purple">
            <span>求解器</span>
            </div></el-col>
          <el-col :span="8"><div class="grid-content bg-purple-light">
            <el-button type="primary" @click="handleRun"  round> 全部运行</el-button>
            </div></el-col>
          <el-col :span="8"><div class="grid-content bg-purple-light">
            <el-button type="primary" @click="handleDownload"  round> 全部下载</el-button>
            </div></el-col>
        </el-row>
        <div v-for="solver in solvers" :key="solver.id" class="data_section">
          <el-row > 
          <el-col :span="8"><div class="grid-content bg-purple">
            <span>{{solver.name}}</span>
            </div></el-col>
          <el-col :span="8"><div class="grid-content bg-purple-light">
            <el-button type="plain" @click="handleRun(solver.id)"  round> 运行</el-button>
            </div></el-col>
          <el-col :span="8"><div class="grid-content bg-purple-light">
            <el-button type="plain" @click="handleDownload"  round> 下载</el-button>
            </div></el-col>
        </el-row>
        </div>
      </el-col>

    </el-row>


</template>

<script >
import { computed, ref } from 'vue'

export default{
  setup(){
    const buttonchose = ref(true)
    const clausenum = ref(1)
    const valnum = ref(1)
    const cnfformula = ref('')
    const error = ref(null)
    const arr = ref([])
    const primiarysolvers = ref([
      {name: "solver1", methods:"abcd",id:1},
      {name: "solver2", methods:"abcd",id:2},
      {name: "solver3", methods:"abcd",id:3}
    ])

    const solvers = computed(()=>{
      return primiarysolvers.value.map(item => {
        return {...item,finish:false}
      })
    })
    /*
    1 2 0
    4 5 6 0
    4 10 -2 -3 0
    */
    const checkvalid = ()=>{
      if(!cnfformula.value){
        error.value = "请输入cnf公式"
        return 
      }
      try{
        //正则表达式匹配数字
        error.value = null
        let nowclause=0,nowvla=0;
        arr.value = cnfformula.value.match(/-?\d+/g); 
        console.log(arr)
        arr.value.forEach(item=>{
          let z = parseInt(item)
          console.log(z)
          if(z == 0){
            nowvla = 0
            nowclause ++
          }
          else{
            nowvla++
            if(Math.abs(z)>valnum.value){
              throw new Error("变量超出限制")
            }
          }
        })
        if(nowclause>clausenum.value){
          throw new Error("子句个数太多")
        }
        else if(nowclause<clausenum.value){
          throw new Error("子句个数不够")
        }
      }
      catch(err){
        error.value = err.message
      }
      
      // console.log(cnfformula.value)
    }
    const handleChange = ()=>{
      console.log("change")
    }
    
    
    return {
      error,cnfformula,solvers,buttonchose,clausenum,valnum,handleChange,checkvalid
    }
  },
  data() {
    return {
      fileList: [{name: 'food.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}, {name: 'food2.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}]
    };
  },
  methods: {
    handleRun(id){
      this.checkvalid()
      if(this.error){
        this.$message.error('请输入正确格式');
      }
      const messa = {
        "clausenum" : this.clausenum,
        "valnum": this.valnum,
        "arr": this.arr
      }
      console.log("run",id)
    },
    handleDownload(){
      console.log("download")
    },
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

<style>
.div-height{
  /* border:1px solid #F00; width:300px; max-height:200px */
  height: 50px;
  vertical-align:middle; /*垂直居中*/
  text-align:center; /*水平居中*/
}
.data_section{
  padding: 10px;
  /* margin-bottom: 10px; */
}
.data_section .data_list{
  text-align: center;
  font-size: 14px;
}
</style>