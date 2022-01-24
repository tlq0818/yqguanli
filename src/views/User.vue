<template>
<div>
  <div style="margin: 10px 0">
    <el-input style="width: 200px" placeholder="请输入姓名" suffix-icon="el-icon-search" v-model="xingming"></el-input>
    <el-input style="width: 200px" placeholder="请输入学院" suffix-icon="el-icon-position" class="ml-5" v-model="xueyuan"></el-input>
    <el-button class="ml-5" type="primary" @click="load">搜索</el-button>
    <el-button class="ml-5" type="warning" @click="reset">重置</el-button>
  </div>
  <div style="margin: 10px 0">
    <el-button type="primary" @click="handAdd">新增 <i class="el-icon-circle-plus-outline"></i></el-button>
    <el-popconfirm
        confirm-button-text='好的'
        cancel-button-text='不用了'
        icon="el-icon-info"
        icon-color="red"
        title="这是一段内容确定批量删除吗？"
        @confirm="delBatch"
    >
      <el-button type="danger" slot="reference">批量删除 <i class="el-icon-remove-outline"></i></el-button>
    </el-popconfirm>
    <el-upload action="http://localhost:9090/user/import" :show-file-list="false" accept="xlsx" :on-success="handleExcelImportSuccess" style="display: inline-block">
    <el-button type="primary">导入 <i class="el-icon-bottom"></i></el-button>
    </el-upload>
    <el-button type="primary" @click="exp">导出 <i class="el-icon-top"></i></el-button>
  </div>
  <el-table :data="tableData" border stripe :header-cell-class-name="headerBg" @selection-change="handleSelectionChange">
    <el-table-column type="selection" width="55"></el-table-column>
    <el-table-column prop="creattime" label="日期" width="140">
    </el-table-column>
    <el-table-column prop="id" label="id" width="120">
    </el-table-column>
    <el-table-column prop="xingming" label="姓名" width="120">
    </el-table-column>
    <el-table-column prop="xuehao" label="学号">
    </el-table-column>
    <el-table-column prop="tiwen" label="体温">
    </el-table-column>
    <el-table-column prop="xueyuan" label="学院">
    </el-table-column>
    <el-table-column label="操作"  width="200" align="center">
      <template slot-scope="scope">
        <el-button type="success" @click="handEdit(scope.row.id,scope.row.xingming,scope.row.xuehao,scope.row.xueyuan,scope.row.tiwen)">编辑 <i class="el-icon-edit"></i></el-button>
        <!--              elementui的table表格当中，slot-scope="scope"，scope.row指的是当前这一行的所有数据-->
        <el-popconfirm
            confirm-button-text='好的'
            cancel-button-text='不用了'
            icon="el-icon-info"
            icon-color="red"
            title="这是一段内容确定删除吗？"
            @confirm="del(scope.row.id)"
        >
          <el-button type="danger" slot="reference">删除 <i class="el-icon-remove-outline"></i></el-button>
        </el-popconfirm>
      </template>
    </el-table-column>
  </el-table>
  <div style="padding: 10px 0">
    <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="pageNum"
        :page-sizes="[2, 4, 6, 8]"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total">
    </el-pagination>
  </div>
  <el-dialog title="学生信息" :visible.sync="dialogFormVisible" width="380px">
    <el-form  label-width="60px" size="small">
      <!--            <el-form-item label="id" >
                    <el-input v-model="form.id" autocomplete="off"></el-input>
                  </el-form-item>-->
      <el-form-item label="姓名">
        <el-input v-model="form.xingming" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="学号" >
        <el-input v-model="form.xuehao" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="体温" >
        <el-input v-model="form.tiwen" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="学院" >
        <el-input v-model="form.xueyuan" autocomplete="off"></el-input>
      </el-form-item>

    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="handsave">确 定</el-button>
      <el-button @click="cancle">取 消</el-button>
    </div>
  </el-dialog>

</div>
</template>

<script>
import request from "@/utils/request";

export default {
  name: "User",
  data(){
    return {
      tableData: [],
      multipleSelection:[],
      total: 0,
      pageNum: 1,
      pageSize: 2,
      xingming: "",
      xueyuan:"",
      tiwen:"",
      xuehao:"",
      form:{},
      dialogFormVisible: false,
      headerBg: 'headerBg'

    }
  },
  created() {
    this.load()
  },
  methods:{
    load() {
      //传数据与接收数据 此时需要解决跨域
      /*fetch("http://localhost:9090/user/page?pageNum="+this.pageNum+"&pageSize="+this.pageSize+"&xingming="+this.xingming+"&xueyuan="+this.xueyuan).then(res=>res.json()).then(res=>{
        console.log(res)
          this.tableData= res.records
            this.total=res.total
      }) 原始传参方法*/
      /*request.get("http://localhost:9090/user/page?pageNum="+this.pageNum+"&pageSize="+this.pageSize+"&xingming="+this.xingming+"&xueyuan="+this.xueyuan).then(res=>{
            console.log(res)
            this.tableData= res.records
            this.total=res.total
      }
      )*/
      request.get("http://localhost:9090/user/page", {
        params: {
          pageNum: this.pageNum,
          pageSize: this.pageSize,
          xingming: this.xingming,
          xueyuan: this.xueyuan
        }
      }).then(res => {
            console.log(res)
            this.tableData = res.data.records
            this.total = res.data.total
          }
      )
    },
    handsave() {
      request.post("http://localhost:9090/user",this.form).then(res => {
        if (res.data) {
          this.$message.success("成功了")
          this.dialogFormVisible = false
          this.load()
        } else {
          this.$message.error("失败了")
          this.dialogFormVisible = false
        }
      })
    },
    del(id){
      request.delete("http://localhost:9090/user/"+id).then(res=>{
        if (res.data) {
          this.$message.success("删除成功了")
          this.load()
        } else {
          this.$message.error("删除失败了")
          this.dialogFormVisible = false
        }
      })
    },

    handEdit(id,xingming,xuehao,xueyuan,tiwen){

      /* this.$set(this.form,'name','mike')*/
      this.$set(this.form,'xingming',xingming)
      this.$set(this.form,'xuehao',xuehao)
      this.$set(this.form,'id',id)
      this.$set(this.form,'tiwen',tiwen)
      this.$set(this.form,'xueyuan',xueyuan)
      this.dialogFormVisible=true
    },

    handleSizeChange(pageSize) {
      /* console.log(pageSize)*/
      this.pageSize = pageSize
      this.load()

    },
    reset() {
      this.xingming = ""
      this.xueyuan = ""
      this.load()
    },
    cancle() {
      this.dialogFormVisible = false
    },
    handAdd(){
      this.dialogFormVisible=true
      this.form={}
    },
    handleSelectionChange(val){
      this.multipleSelection=val
    },
    delBatch(
    ){
      let ids=this.multipleSelection.map(v=>v.id)
      request.post("http://localhost:9090/user/batch",ids).then(res=>{
        if (res.data) {
          this.$message.success("批量删除成功了")
          this.load()
        } else {
          this.$message.error("删除失败了")
          this.dialogFormVisible = false
        }
      })
    },
    exp(){
      window.open("http://localhost:9090/user/export")
    },
    handleExcelImportSuccess(){
      this.$message.success("文件导入成功")
      this.load()
    },
    handleCurrentChange(pageNum){
      /*  console.log(pageNum)
  */
      this.pageNum=pageNum
      this.load()
    }
  }

}
</script>

<style>
.headerBg {
  background: #eee!important;
}
</style>
