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
          <el-upload
            class="upload-demo"
            action="https://jsonplaceholder.typicode.com/posts/"
            :on-preview="handlePreview"
            :on-remove="handleRemove"
            :before-remove="beforeRemove"
            multiple
            :limit="3"
            :on-exceed="handleExceed"
            :file-list="fileList"
          >
            <el-button type="primary">Click to upload</el-button>
            <template #tip>
              <div class="el-upload__tip">
                jpg/png files with a size less than 500KB.
              </div>
            </template>
          </el-upload>
        </div>
      </el-aside>
      <el-main>Main</el-main>
    </el-container>
  </div>

</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { ElMessage, ElMessageBox } from 'element-plus'
import type { UploadProps, UploadUserFile } from 'element-plus'

export default {
  name: 'HomeView',
  // data(){
  //   return {
  //     buttonchose:true,
  //   }
  // },
  // methods:{
  // },
  // components: {

  // },
  setup(){
    const buttonchose = ref(true)
    const clausenum = ref(1)
    const valnum = ref(1)
    const textarea = ref('')
    const handleChange = (value) => {
      console.log(value)
    }
    const fileList = ref<UploadUserFile[]>([
      {
        name: 'food.jpeg',
        url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100',
      },
      {
        name: 'food2.jpeg',
        url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100',
      },
    ])

    const handleRemove: UploadProps['onRemove'] = (file, uploadFiles) => {
      console.log(file, uploadFiles)
    }

    const handlePreview: UploadProps['onPreview'] = (uploadFile) => {
      console.log(uploadFile)
    }

    const handleExceed: UploadProps['onExceed'] = (files, uploadFiles) => {
      ElMessage.warning(
        `The limit is 3, you selected ${files.length} files this time, add up to ${
          files.length + uploadFiles.length
        } totally`
      )
    }

    const beforeRemove: UploadProps['beforeRemove'] = (uploadFile, uploadFiles) => {
      return ElMessageBox.confirm(
        `Cancel the transfert of ${uploadFile.name} ?`
      ).then(
        () => true,
        () => false
      )
    }
    return {textarea,valnum,clausenum,handleChange,buttonchose,fileList,handleRemove,handlePreview,handleExceed,beforeRemove}
  },
  
}
</script>



