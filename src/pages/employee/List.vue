<template>
    <div>
        <center>员工管理</center><br/>
        <el-button type="primary" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">批量删除</el-button>

        <el-table :data="employees">

            <el-table-column fixed="left" prop="id" label="编号"></el-table-column>
            <el-table-column fixed="left" prop="username" label="用户名"></el-table-column>
            <el-table-column prop="realname" label="姓名"></el-table-column>
            <el-table-column prop="gender" label="性别"></el-table-column>
            <el-table-column prop="idCard" width="200" label="身份证号"></el-table-column>
            <el-table-column prop="telephone" width="200" label="手机号"></el-table-column>
            <el-table-column prop="bankCard" width="200" label="银行卡号"></el-table-column>

            <el-table-column fixed="right" label="操作">
                <!--//slot接受当前行信息 -->
                <template v-slot="slot">

                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdataHandler(slot.row)">修改</a>
                </template>
            </el-table-column>
        </el-table>

        <el-pagination
            layout="prev, pager, next"
            :total="50">
        </el-pagination>

        <el-dialog
        :title="title"
        :visible.sync="visible"
        width="60%"
        >

        ---{{form}}
        <el-form :model="form" label-width="80px">
            <el-form-item label="用户名">
                <el-input v-model="form.username"></el-input>
            </el-form-item>
            <el-form-item label="密码">
                <el-input type="password" v-model="form.password"></el-input>
            </el-form-item>
            <el-form-item label="姓名">
                <el-input v-model="form.realname"></el-input>
            </el-form-item>
            <el-form-item label="性别">
                <el-radio-group v-model="form.gender">
                    <el-radio label="男">男</el-radio>
                    <el-radio label="女">女</el-radio>
                </el-radio-group>
                <el-input v-model="form.gender"></el-input>
            </el-form-item>
            <el-form-item label="手机号">
                <el-input v-model="form.telephone"></el-input>
            </el-form-item>
            <el-form-item label="身份证号">
                <el-input v-model="form.idCard"></el-input>
            </el-form-item>
            <el-form-item label="银行卡号">
                <el-input v-model="form.bankCard"></el-input>
            </el-form-item>
        </el-form>

        <span slot="footer" class="dialog-footer">
            <el-button @click="closeMedalHandler" size="small">取 消</el-button>
            <el-button type="primary" @click="submitHandler" size="small">确 定</el-button>
        </span>
        </el-dialog>

    </div>
</template>
<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
        methods:{
            //重载
         loadData(){
             let url = "http://localhost:6677/waiter/findAll"
             request.get(url).then((response)=>{
                 this.employees = response.data;
             });
        },

        toAddHandler(){
            this.title = "录入员工信息"
            this.visible = true;
        },
        toUpdataHandler(row){
            this.title = "修改员工信息"
            this.visible = true;
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
            message: '删除成功!'+id
          });
        })
        },   
        closeMedalHandler(){
            this.visible=false;
        },
        submitHandler(){
            let url = "http://localhost:6677/waiter/saveOrUpdate"
            request({
                url,
                method:"POST",
                headers:{
                    "Content-Typpe":"application/x-www-form-urlencoded"
                },
                data:querystring.stringify(this.form)
            }).then((response)=>{
                this.closeMedalHandler();
                    this.$message({
                    type:"success",
                    message:response.message
                })
                this.loadData();
            })
        }
    },
    created(){
        this.loadData();
    },

    //用于存放网页中显示的数据
    data(){
        return{
            title:"录入员工信息",
            visible:false,
            employees:[],
            form:{
                type:"waiter"
            }
        }
    }
}
</script>
<style scoped>

</style>
