<template>
  <el-menu :default-openeds="['1', '3']" style="min-height: 100%; overflow-x: hidden"
           background-color="rgb(255, 110, 255)"
           text-color="#fff"
           active-text-color="#ffd04b"
           :collapse-transition="false"
           :collapse="isCollapse"
           router
  >
    <div style="height: 60px; line-height: 60px; text-align: center">
      <img src="../assets/logo.png" alt="" style="width: 20px; position: relative; top: 5px; right: 5px">
      <b style="color: white" v-show="logoTextShow">疫情管理系统</b>
    </div>
    <el-menu-item index="/huanying">
      <i class="el-icon-trophy"></i>
      <span slot="title">欢迎</span>
    </el-menu-item>
    <el-menu-item index="/home">
      <i class="el-icon-message"></i>
      <span slot="title">主页</span>
    </el-menu-item>
    <el-submenu index="2">
      <template slot="title">
        <i class="el-icon-menu"></i>
        <span slot="title">系统管理</span>
      </template>
      <el-menu-item index="/user">
        <i class="el-icon-s-custom"></i>
        <span slot="title">用户管理</span>
      </el-menu-item>
      <el-menu-item index="/file">
        <i class="el-icon-document"></i>
        <span slot="title">核酸照片管理</span>
      </el-menu-item>
    </el-submenu>
  </el-menu>
</template>

<script>
export default {
  name: "Aside",
  props: {
    isCollapse: Boolean,
    logoTextShow: Boolean
  }
}
</script>

<style scoped>

</style>