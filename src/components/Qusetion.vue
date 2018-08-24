<template>
   <el-container>
      <el-header>
        <el-row>     
          <el-button type="primary" @click="reverseMessage">生成题目</el-button>
          <el-button type="success">显示答案</el-button>
        </el-row>
      </el-header>
      <el-main> 
        <el-row>
          
            <span name='frac' fontsize='12' >2/15</span><span fontsize='10'>+1</span>
          
          <div name='equation' fontsize='12'>3x+4=12,4y+5=3x-6,dz+3=9</div>
        </el-row>
        <el-row>
          <el-radio-group v-model="radio1">
            <el-radio-button label="困难">困难</el-radio-button>
            <el-radio-button label="中等">中等</el-radio-button>
            <el-radio-button label="简单">简单</el-radio-button>
          </el-radio-group>
        </el-row>
        
        <el-row>
          <el-radio-group v-model="radio2">
            <el-radio-button label="整数"></el-radio-button>
            <el-radio-button label="小数"></el-radio-button>
            <el-radio-button label="分数"></el-radio-button>
          </el-radio-group>
        </el-row>
        <hr>
        <ul id="example-2">
          <li v-for="(data, index) in tableData" :key="data.question">
            {{ index }}.{{data.question}}{{data.answer}}
          </li>
        </ul>

        <el-row>
        <el-table
          :data="tableData"  
          style="width: 100%">
          <el-table-column
            label="题目"
            >
           
            <template slot-scope="scope">
             
              <span name="frac" style="margin-left: 10px">{{ scope.row.question }}</span>
            </template>
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
import {Formula} from '../plugin/formula.js'



export default {
  data() {
    return {
      radio1: "1",
      radio2: "1",
      checked: true,
      tableData: [{
          question: "1/22-6",
          answer: '342'
        }, {
          question: '1/2+2/3×2÷7',
          answer: '144'
        }, {
          question: '999-222-111=',
          answer: '666'
        }]
      }
  },
  methods:{reverseMessage: function () {
      var arr = this.tableData[1].question.split(/\+|-|×|÷/)
      alert(arr.length)
       alert(arr)
       var arr = this.tableData[1].question.split(/[^\d/]/)
      alert(arr.length)
       alert(arr)
    }},
  mounted: function () {
  this.$nextTick(function () {
    // Code that will run only after the
    // entire view has been rendered
    var formula=new Formula();
    formula.processAll();
  })
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
