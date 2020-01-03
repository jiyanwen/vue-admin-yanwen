<template>
    <div>
        <!--按钮-->
        <el-button type="primary" size="smell" @click="toAddHandler" >添加</el-button>
        <el-button type="danger" size="smell" >批量删除</el-button>
         <!--按钮结束-->
          <!--表格-->
        <el-table :data="customers">
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="realname" label="姓名"></el-table-column>
            <el-table-column prop="gender" label="性别"></el-table-column>
            <el-table-column prop="telephone" label="联系方式"></el-table-column>
             <el-table-column prop="id-card" width="200" label="联系方式"></el-table-column>
            <el-table-column label="操作">
                <template v-slot="slot">
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdateHandler">修改</a>

                </template>

            </el-table-column>
        </el-table>
         <!--表格结束-->
         <!--分页开始-->
         <el-pagination
        layout="prev, pager, next"
        :total="50">
         </el-pagination>
         <!--分页结束-->
         <!--模态框-->
         <el-dialog
        title="录入顾客信息"
        :visible.sync="visible"
        width="60%">
        ---{{form}}
            <el-form :model="form" label-width="80px">
                <el-form-item label="名称">
                <el-input v-model="form.realname"></el-input> 
                </el-form-item> 
            </el-form>

            <el-form :model="form" label-width="80px">
                 <el-form-item label="价格">
                <el-input type="password" v-model="form.password"></el-input>
                </el-form-item>
            </el-form>  

             <el-form :model="form" label-width="80px">
                 <el-form-item label="所属栏目">
                <el-input type="password" v-model="form.password"></el-input>
                </el-form-item>
            </el-form>

             <el-form :model="form" label-width="80px">
                 <el-form-item label="介绍">
                <el-input type="password" v-model="form.password"></el-input>
                </el-form-item>
            </el-form> 

             <el-form :model="form" label-width="80px">
                 <el-form-item label="产品主图">
                <el-upload
                    class="upload-demo"
                    action="https://jsonplaceholder.typicode.com/posts/"
                    :on-preview="handlePreview"
                    :on-remove="handleRemove"
                    :file-list="fileList"
                    list-type="picture">
                    <el-button size="small" type="primary">点击上传</el-button>
                    <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
                </el-upload>
                </el-form-item>
            </el-form>          

        <span slot="footer" class="dialog-footer">
            <el-button size="small" @click="closeMedalHandler">取 消</el-button>
            <el-button size="small" type="primary" @click="submitHander">确 定</el-button>
         </span>
        </el-dialog>
    </div>
</template>

<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
    //用于存放网页中需要存放的方法
    methods:{
        loadData(){
             let url="http://localhost:6677/product/findAll"
             request.get(url).then((response)=>{//指向外部this
             //将查询结果设置到customer
             this.customers=response.data;
             })
        },

        submitHander(){
            let url="http://localhost:6677/product/saveOrUpdate"
                request({
                  url,
                  method:"POST",
                  headers:{
                   "Content-Type":"application/x-www-form-urlencoded"
                    },
                data:querystring.stringify(this.form)
                }).then((response)=>{
                 this.closeMedalHandler();
                 this.loadData();
                 this.$message({
                      type:"success",
                    message:response.message
                })
            })
        },
    
        toDeleteHandler(id){
            //确认
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

        toUpdateHandler(){
            this.visible=true;

        },
        closeMedalHandler(){
            this.visible=false;

        },
        toAddHandler(){
            this.visible=true;
        },

    },
    //用于存放要向网页中显示的数据
    data(){
        return{
            visible:false,
            customers:[],
            form:{
             type:"product"
            }
        }
    },
   
    created(){
        // this为当前vue实例对象
        // vue实例创建完毕 
        this.loadData();
 
    }
}
</script>

<style scoped>

</style>