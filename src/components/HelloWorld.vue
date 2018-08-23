<template>
   <el-container>
      <el-main>
        <el-row>     
          <el-button type="primary">主要按钮</el-button>
          <el-button type="success">成功按钮</el-button>
        </el-row>
        <el-row>
          <el-radio-group v-model="radio3">
            <el-radio-button label="上海"></el-radio-button>
            <el-radio-button label="北京"></el-radio-button>
            <el-radio-button label="广州"></el-radio-button>
            <el-radio-button label="深圳"></el-radio-button>
          </el-radio-group>
        </el-row>
      </el-main>
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
  

  .el-main {
    background-color: #E9EEF3;
    color: #333;
    text-align: center; 
    line-height: 60px;
  }
 
 
</style>
