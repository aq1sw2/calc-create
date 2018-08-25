<template>
   <el-container>
      <el-header>
        <el-row>     
          <el-button type="primary" @click="createList">生成题目</el-button>
          <el-button type="success" @click="reverseMessage">显示答案</el-button>
        </el-row>
      </el-header>
      <el-main> 
        <!--
        <el-row>
          <span name='frac' fontsize='12' >2/15</span><span fontsize='10'>+1</span>
          <div name='equation' fontsize='12'>3x+4=12,4y+5=3x-6,dz+3=9</div>
        </el-row>
        -->
        <el-row>
          <el-radio-group v-model="calcNum">
            <el-radio-button label="6">困难</el-radio-button>
            <el-radio-button label="5">中等</el-radio-button>
            <el-radio-button label="4">简单</el-radio-button>
          </el-radio-group>
        </el-row>
        
        <el-row>
          <el-radio-group v-model="calcType">
            <el-radio-button label="1">整数</el-radio-button>
            <el-radio-button label="2">小数</el-radio-button>
            <el-radio-button label="3">分数</el-radio-button>
          </el-radio-group>
        </el-row>
        
        <!--
        <ul id="example-2">
          <li v-for="(data, index) in tableData" :key="data.question">
            {{ index }}.{{data.question}}{{data.answer}}
          </li>
        </ul>
        -->
        <el-row>
        <el-table
          :data="tableData"  
          height="250"
          style="width: 100%">
          <el-table-column
            label="题目"
            prop="question"
            align ="left"
            >
            <!--<template slot-scope="scope">
             
              <span name="frac" style="margin-left: 10px">{{ scope.row.question }}</span>
            </template>-->
          </el-table-column>
          <!--<el-table-column
            prop="answer"
            label="答案"
            >
          </el-table-column>-->
        </el-table>
        </el-row>
      </el-main>
  </el-container>
</template>

<script>
//import {Formula} from '../plugin/formula.js'

export default {
  data() {
    return {
      calcNum: "4",
      calcType: "1",
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
  methods:{
    reverseMessage: function () {
      // var arr = this.tableData[1].question.split(/\+|-|×|÷/)
      // alert(arr.length)
      //  alert(arr)
      //  var arr = this.tableData[1].question.split(/[^\d/]/)
      // alert(arr.length)
      //  alert(arr)
    },
    createList:function(){

      this.tableData = new Array()
      //setting from UI
      let defaultMax = this.getdefaultMax()
      let parentheses = this.getParenthese() 

      //anyway,create 10 question everytime
      let count = 10
      for (let i=0;i<count;i++)
      { 
          let date = {}
          date.question = this.createQ(defaultMax,parentheses)
          date.answer = this.createA(date.question)
          this.tableData.push(date) 
      }
    },
    createQ:function(defaultMax,parentheses){
      
      //init operator
      //1 +  2 -  3 * 4 /
      let operator = new Array()
      for (let i=0;i<this.calcNum-2;i++)
      { 
          operator.push(this.rndInt(1,4))
      }
      //init operator weight
      let weight = new Array()
      for (let i=0;i<operator.length;i++)
      { 
          weight.push(this.getDefaultW(operator[i]))
      }
      
      //deal with parentheses
      let up_weight_count = parentheses
      let left_weight = 0
      let right_weight = 0
      for (let i=0;i<weight.length;i++)
      { 
         if(up_weight_count)
         {
            if(i>0)
            {
               left_weight = weight[i-1]
            }else
            {
               left_weight = 0
            }

            if(i<weight.length-1)
            {
               right_weight = weight[i+1]
            }else
            {
               right_weight = 0
            }
            //up weight only when has a less weight than right and left
            if(weight[i]<left_weight || weight[i]<right_weight) 
            {
                up_weight_count-- 
                weight[i] = weight[i] + 1  
                //skip next i when up weight
                i++
            }
         }else
         {
           break
         } 
      }
      
      //deal with right parentheses to close
      let need_parentheses = false
      let calc = ""
      for (let i=0;i<this.calcNum;i++)
      { 
          //left parentheses
          if (i<this.calcNum-1)
          {
             if (this.getDefaultW(operator[i])<weight[i])
             {
                calc=calc+"("
                need_parentheses = true
             }
          }
          let main_num = this.rndInt(1,defaultMax)

          switch(parseInt(this.calcType))
          {
              case 2:
                calc = calc + main_num + "." + this.rndInt(1,defaultMax)
                break;
              case 3:
                calc = calc + this.rndInt(1,main_num-1) + "/" + main_num
                break;
              default:
                calc = calc+main_num
          }

          if(i > 0)
          {
             if (this.getDefaultW(operator[i-1])<weight[i-1]&&need_parentheses)
             {
                calc = calc+")"
                need_parentheses = false
             }
          }
        
          if(i<this.calcNum-1) 
          {
              calc = calc+ this.getPrintOper(operator[i])
          }
      }

      return calc

    },
    createA:function(question){
      //TODO
      //anyway,create 10 question everytime
      let count = 10
      for (let i=0;i<count;i++)
      { 
         // document.write(cars[i] + "<br>");
      }
      return ""
    },
    getDefaultW:function(operator)
    {
        let weight = 1;
        if (operator>2)
        {
          weight = 2
        }
        return weight
    },
    rndInt :function (n, m){
        let random = Math.floor(Math.random()*(m-n+1)+n)
        return random
    },
    getdefaultMax :function (){
        //4:max 20 |5:max 30|6:max 40
        let max = this.rndInt(1,(parseInt(this.calcNum)-2)*10)
        return max
    },
    getParenthese :function (){
        let parentheses = parseInt(this.calcNum)%3-1
        return parentheses
    },
    getPrintOper:function(operator)
    {   
        let printOper = "+"
        switch(operator)
        {
          case 2:
            printOper ="-"
            break;
          case 3:
            printOper ="×"
            break;
          case 4:
            printOper ="÷"
            break;
          default:
            printOper ="+"
        }
        return printOper
    }
  },
  mounted: function () {
  this.$nextTick(function () {
    // Code that will run only after the
    // entire view has been rendered
    // this is code is used for show fraction
    //var formula=new Formula();
    //formula.processAll();
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
