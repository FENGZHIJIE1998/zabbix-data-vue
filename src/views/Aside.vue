<template>
  <el-row class="tac">
    <el-col :span="24">
      <el-menu
        default-active="1"
        class="el-menu-vertical-demo"
        background-color="#fff"
        text-color="#000"
        active-text-color="#ffd04b"
        router
      >
        <el-menu-item index="1" route="/">
          <i class="el-icon-location"></i>
          <span slot="title">首页</span>
        </el-menu-item>
        <el-submenu index="2">
        <template slot="title">
          <i class="el-icon-menu"></i>
          <span>数据</span>
        </template>
          <el-menu-item index="1-1" route="/excel">
            <i class="el-icon-document"></i>
            <span slot="title">导出</span>
          </el-menu-item>
          <el-menu-item index="1-2" route="/histroy">
            <i class="el-icon-folder-opened"></i>
            <span slot="title">历史</span>
          </el-menu-item>
        </el-submenu>
        <el-menu-item index="4" route="/item">
          <i class="el-icon-setting"></i>
          <span slot="title">设置</span>
        </el-menu-item>
      </el-menu>
    </el-col>
  </el-row>
</template>


<script>
export default {
  methods: {}
};
</script>



<style>
</style>