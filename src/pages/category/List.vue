<template>
  <div>
    <h2>栏目管理</h2>
    <!-- 按钮 -->
    <el-button size="samll" type="primary" @click="toAddHandel">添加</el-button> 
    <el-button size="small" type="danger">批量删除</el-button>
    <!-- /按钮 -->
    <!-- 表格 -->
    <el-table :data="category">
      <el-table-column prop="id" label="编号"></el-table-column>
      <el-table-column prop="name" label="栏目名称"></el-table-column>
      <el-table-column width="200" prop="num" label="序号"></el-table-column>
            <el-table-column prop="parentId" label="父栏目"></el-table-column>
      <el-table-column label="操作">
        <template v-slot="slot">
          <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
          <a href="" @click.prevent="toUpdateHandler(slot.row)">修改</a>
        </template>
      </el-table-column>
    </el-table>
    <!-- /表格结束 -->
    <!-- 分页开始 -->
    <el-pagination layout="prev, pager, next" :total="50"></el-pagination>
    <!-- /分页结束 -->
    <!-- 模态框 -->
    <el-dialog
      :title="title"
      :visible.sync="visible"
      width="60%">
      <el-form ref:model="form" label-width="80px">
        <el-form-item label="编号">
          <el-input v-model="form.id"></el-input>
          <!-- v-model双向数据绑定 -->
        </el-form-item>
        <el-form-item label="栏目名称">
          <el-input v-model="form.name"></el-input>
        </el-form-item> 
        <el-form-item label="序号">
          <el-input  v-model="form.num"></el-input>
        </el-form-item> 
        <el-form-item label="父栏目">
          <el-input  v-model="form.parentId"></el-input>
        </el-form-item> 
      </el-form>

      <span slot="footer" class="dialog-footer">
        <el-button size="small" @click="closeModalHandler">取 消</el-button>
        <el-button size="small" type="primary" @click="submitHandler">确 定</el-button>
      </span>
    </el-dialog>
    <!-- /模态框 -->

  </div>
</template>
<script>
import request from'@/utils/request'
import querystring from 'querystring'
export default {
    created(){
    // this为当前vue实例对象
    // vue实例创建完毕 
    this.loadData()

  },
    data(){
        return{
      visible:false,
      category:[],
      form:{
        type:"category"
        }
    }  
    
},

methods:
{
    loadData(){
      let url = "http://localhost:6677/category/findAll"
      request.get(url).then((response)=>{
        // 将查询结果设置到customers中，this指向外部函数的this
        this.category = response.data;
      })
    },
    submitHandler(){
        //   调用后台接口，并携带参数
        let url="http://localhost:6677/category/saveOrUpdate"
        request({
            url,
            method:"POST",
            headers:{
                "Content-Type":"application/x-www-form-urlencoded"
            },
            data:querystring.stringify(this.form)
            // 交互完毕
        }).then((response)=>{
            // 模态框关闭
            this.closeModalHandler();
            this.$loadData();
            this.$message({
                type:"success",
                message:response.message
            })
        })
      },

    toDeleteHandler(id){
         this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        })

    },
        toAddHandel(){
        this.visible = true;
    },
    toUpdateHandler(row){
        this.visible = true;

    },
    closeModalHandler(){
      this.visible = false;
    }
}
}

</script>
<style scoped>

</style>

