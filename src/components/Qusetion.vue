<template>
   <el-container>
      <el-header>
        <el-row>     
          <el-button type="primary">生成题目</el-button>
          <el-button type="success">显示答案</el-button>
        </el-row>
      </el-header>
      <el-main> 
        <el-row>
          <el-radio-group v-model="radio1">
            <el-radio-button label="困难"></el-radio-button>
            <el-radio-button label="中等"></el-radio-button>
            <el-radio-button label="简单"></el-radio-button>
          </el-radio-group>
        </el-row>
        <hr>
        <el-row>
          <el-radio-group v-model="radio2">
            <el-radio-button label="整数"></el-radio-button>
            <el-radio-button label="小数"></el-radio-button>
            <el-radio-button label="分数"></el-radio-button>
          </el-radio-group>
        </el-row>
        <el-row>
        <el-table
          :data="tableData"
          style="width: 100%">
          <el-table-column
            prop="question"
            label="题目"
            >
          </el-table-column>
          <el-table-column
            prop="answer"
            label="答案"
            >
          </el-table-column>
        </el-table>
        </el-row>
      </el-main>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      radio1: "1",
      radio2: "1",
      checked: true,
      tableData: [{
          question: '1-123+1111-123=',
          answer: '342'
        }, {
          question: '1-123+(1111-123)=',
          answer: '144'
        }, {
          question: '999-222-111=',
          answer: '666'
        }]
      }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .el-header, .el-footer {
    background-color: #B3C0D1;
    color: #333;
    text-align: center;
    line-height: 60px;
  }

.el-main {
  background-color: #e9eef3;
  color: #333;
  text-align: center;
  line-height: 60px;
}

  body > .el-container {
    margin-bottom: 20px;
  }
</style>
