<script setup>
// TODO: 编辑
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { defineEmits } from 'vue'
import { ElMessage } from 'element-plus'

const emit = defineEmits(['updata'])
const data = ref({})
// 弹框开关
const dialogVisible = ref(false)
const open = (row) => {
  dialogVisible.value = true
  // console.log(row);
  data.value = { ...row } // 把传来的row记录一下 发请求用
}
defineExpose({ open })

// 点确认 更新数据 -> 通知父组件刷新页面
const edit = async () => {
  await axios.patch(`/edit/${data.value.id}`,
    data.value
  )
  dialogVisible.value = false
  emit('updata')
  ElMessage.success('编辑成功')
}



</script>

<template>
  <el-dialog v-model="dialogVisible" title="编辑" width="400px">
    <el-form label-width="50px">
      <el-form-item label="姓名">
        <el-input placeholder="请输入姓名" v-model="data.name" />
      </el-form-item>
      <el-form-item label="籍贯">
        <el-input placeholder="请输入籍贯" v-model="data.place" />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="edit()">确认</el-button>
      </span>
    </template>
  </el-dialog>
</template>

<style scoped>
.el-input {
  width: 290px;
}
</style>
