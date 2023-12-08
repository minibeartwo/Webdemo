<template>
  <div class="table-box">
    <div class="title-box">
      <p>
      <h2>
        CRUD
      </h2>
      </p>
    </div>
    <div class="input-box">
      <el-input v-model="queryinput" placeholder="请输入要搜索的姓名" @input="handlequeryname"/>
      <div class="btn-box">
        <el-button type="primary" @click="handleadd">新增</el-button>
        <el-button type="danger" @click="handleDell" v-if="multipleSelection.length > 0">删除多选</el-button>
      </div>
    </div>
    <el-table border ref="multipleTableRef" :data="tableData" style="width: 100%"
      @selection-change="handleSelectionChange">
      <el-table-column type="selection" width="55" />
      <el-table-column prop="name" label="姓名" width="100" />
      <el-table-column prop="email" label="邮箱" width="120" />
      <el-table-column prop="phone" label="电话" width="120" />
      <el-table-column prop="state" label="状态" width="120" />
      <el-table-column prop="address" label="地址" width="250" />
      <el-table-column fixed="right" label="操作" width="120">
        <template #default="scope">
          <el-button link type="primary" size="small" @click="handledel(scope.row)" style="color: red;">删除</el-button>
          <el-button link type="primary" size="small" @click="handleedit(scope.row)">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog v-model="dialogFormVisible" :title="dialogtype === 'add' ? '新增' : '编辑'">
      <el-form :model="tableform">
        <el-form-item label="姓名" :label-width="formLabelWidth">
          <el-input v-model="tableform.email" autocomplete="off" />
        </el-form-item>
        <el-form-item label="邮箱" :label-width="formLabelWidth">
          <el-input v-model="tableform.name" autocomplete="off" />
        </el-form-item>
        <el-form-item label="电话" :label-width="formLabelWidth">
          <el-input v-model="tableform.phone" autocomplete="off" />
        </el-form-item>
        <el-form-item label="地址" :label-width="formLabelWidth">
          <el-input v-model="tableform.address" autocomplete="off" />
        </el-form-item>
      </el-form>
      <template #footer>
        <span class="dialog-footer">
          <el-button type="primary" @click="handleconfig">
            确认
          </el-button>
        </span>
      </template>
    </el-dialog>
  </div>
</template>
<script setup>
import { ref } from 'vue';
let queryinput = ref("");
let multipleSelection = ref([]);
let dialogFormVisible = ref(false);
let dialogtype = ref("add");

const formLabelWidth = ref(50);
let tableform = ref({
  id: "",
  name: "",
  email: "",
  phone: "",
  address: ""
})
let tableData = ref([{
  id: "1",
  name: 'Tom1',
  email: "123@qq.com",
  state: 'California',
  address: 'No. 189, Grove St, Los Angeles',
  phone: "123456789",
},
{
  id: "2",
  name: 'Tom2',
  state: 'California',
  email: "123@qq.com",
  address: 'No. 189, Grove St, Los Angeles',
  phone: "123456789",
},
{

  id: "3",
  name: 'tom',
  state: 'California',
  email: "123@qq.com",
  address: 'No. 189, Grove St, Los Angeles',
  phone: "123456789",
},
{
  id: "4",
  name: 'xiaoming',
  state: 'California',
  email: "123@qq.com",
  address: 'No. 189, Grove St, Los Angeles',
  phone: "123456789",
},]);
let tableDatacopy=Object.assign(tableData.value);
const handlequeryname=(val)=>{
  if(val.length>0){
    tableData.value=tableData.value.filter(item=>item.name.match(val));
  }else{
    tableData=tableDatacopy;
  }
}
const handleedit = (row) => {
  dialogFormVisible.value = true;
  tableform.value = { ...row }
  dialogtype.value = "edit";
}
const handleDell = () => {
  multipleSelection.forEach(id => {
    handledel({ id });
  })
  multipleSelection = []
}
const handleadd = () => {
  dialogtype.value = "add";
  dialogFormVisible.value = true;
  tableform.value = {};
}
const handledel = (row) => {
  let id=row.id;
  let index = tableData.value.findIndex(item => item.id === id);
  tableData.value.splice(index, 1);
}
const handleSelectionChange = (val) => {
  multipleSelection = [];
  val.forEach(item => {
    multipleSelection.push(item.id);
  });
}
const handleconfig = () => {
  if (dialogtype.value === "add") {
    dialogFormVisible.value = false;
    tableData.value.push({
      id: (tableData.length + 1).toString(),
      ...tableform.value,
    })
  }else if(dialogtype.value==="edit"){
    let index=tableData.value.findIndex(item=>item.id===tableform.value.id);
    tableData.value[index]=tableform.value
    dialogFormVisible.value=false;
  }

}
</script>

<style scoped>
.table-box {
  width: 800px;
  margin: 200px auto;
}

.title-box {
  text-align: center;
}

.input-box {
  display: flex;
  justify-content: space-between;
  margin-bottom: 30px;
}

.input-box .el-input {
  left: 0%;
  width: 200px;
}
</style>