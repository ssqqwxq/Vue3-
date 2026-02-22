<script setup>
import Edit from './components/Edit.vue'
import axios from 'axios';
import { ref, onMounted } from 'vue'
import { ElMessage } from 'element-plus'

// TODO: 列表渲染
const list = ref([])
const getList = async () => {
  const res = await axios.get('/list')
  list.value = res.data
}
onMounted(() => {
  getList()
})

// TODO: 删除功能
const del = async (id) => {
  await axios.delete(`/del/${id}`)
  // console.log(id);
  ElMessage.success('删除成功')
  getList()
}

// TODO: 编辑功能
const tableRef = ref()

const edit = (row) => {
  tableRef.value.open(row) //打开弹窗顺道传个row

}
</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default="{ row }">
          <el-button type="primary" link @click="edit(row)">编辑</el-button>
          <el-button type="danger" link @click="del(row.id)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit ref="tableRef" @updata="getList" />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
