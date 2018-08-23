<template>
  <el-container>
    <el-header>
      <el-row>
        <el-radio v-model="radio" label="1">备选项</el-radio>
        <el-radio v-model="radio" label="2">备选项</el-radio>
        <el-radio v-model="radio" label="3">备选项</el-radio>
      </el-row>
      <el-row>
        <el-radio v-model="radio" label="1">备选项</el-radio>
        <el-radio v-model="radio" label="2">备选项</el-radio>
        <el-radio v-model="radio" label="3">备选项</el-radio>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="12"><el-button type="primary" plain>主要按钮</el-button></el-col>
        <el-col :span="12"><el-checkbox v-model="checked">备选项</el-checkbox></el-col>
      </el-row>
    </el-header>
    <el-main>Main</el-main>
  </el-container>
</template>

<script>
export default {
    data () {
      return {
        radio: '1',
        checked: true
      };
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .el-header, .el-footer {
    background-color: #B3C0D1;
    color: #333;
    text-align: center;
  }

  .el-main {
    background-color: #E9EEF3;
    color: #333;
    text-align: center; 
  }
 
 
</style>
