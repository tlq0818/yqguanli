<template>
<el-card style="width: 500px;">
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
    <el-form-item>
    <el-button @click="save">确 定</el-button>
  </el-form-item>
  </el-form>
</el-card>
</template>

<script>
import request from "@/utils/request";

export default {
  name: "person",
  data(){
    return{
      form: {},
      user: localStorage.getItem("user")?JSON.parse(localStorage.getItem("user")) : {}
    }
  },
  created() {
    this.request.get("http://localhost:9090/user/xingming/"+this.user.xingming).then(res =>{
    if(res.code==="200") {
      this.$set(this.form,'xingming',res.data.xingming)
      this.$set(this.form,'xuehao',res.data.xuehao)
      this.$set(this.form,'id',res.data.id)
      this.$set(this.form,'tiwen',res.data.tiwen)
      this.$set(this.form,'xueyuan',res.data.xueyuan)
    }
    })
  },
  methods: {
    save(){
      request.post("http://localhost:9090/user",this.form).then(res => {
        if (res.data) {
          this.$message.success("成功了")
        } else {
          this.$message.error("失败了")

        }
      })
    }
  },
  handleAvatarSuccess(res) {
    this.form.avatarUrl = res
  }
}
</script>

<style>

</style>