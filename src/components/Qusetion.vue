<template>
   <el-container>
      <el-header>
        <el-row>     
          <el-button type="primary" @click="createList">生成题目</el-button>
          <!--<el-button type="success" @click="reverseMessage">显示答案</el-button>-->
          <el-button type="success">耗时:{{cost|times}}</el-button>
        </el-row>
      </el-header>
      <el-main> 
        <!--
        <el-row>
          <span name='frac' fontsize='12' >2/15</span><span fontsize='10'>+1</span>
          <div name='equation' fontsize='12'>3x+4=12,4y+5=3x-6,dz+3=9</div>
        </el-row>
        
        <el-row>
          <el-tag type="danger">玩耍的日子，剩余:{{leftTime|dates}}</el-tag>
        </el-row>-->
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
          height="400"
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
import {reductionofAFraction} from '../plugin/fractions.js'
export default {
  data() {
    return {
      start:0,
      cost:0,
      leftTime:0,
      calcNum: "4",
      calcType: "1",
      tableData: [{
          question: '1/22-6',
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
  filters: 
  {
      dates:function (value) {
          let days =  parseInt(value/1000/60/60/24)
          let hours = parseInt(value/1000/60/60%24)
          let minutes = parseInt(value/1000/60%60)
          let seconds = parseInt(value/1000%60)
          let msecs = parseInt(value%1000)
          //return days + '天'+hours + '时' + minutes + '分' +seconds+ '秒'+msecs
          return days + '天'+hours + '时' + minutes + '分' +seconds+ '秒'
      },
      times:function (value) {
          let hours = parseInt(value/1000/60/60%24)
          let minutes = parseInt(value/1000/60%60)
          let seconds = parseInt(value/1000%60)
          let msecs = parseInt(value%1000)
          //return hours + '时' + minutes + '分' +seconds+ '秒'+msecs
          return hours + '时' + minutes + '分' +seconds+ '秒'
      }
  },
  methods:{
    countdown:function()
    {
       let endDate = new Date()
       endDate.setFullYear(2023,8,1)
       endDate.setHours(0,0,0,0)
       return endDate- new Date()
    },
    countup:function()
    {  
       if(this.start >0)
       {
         return new Date()-this.start
       }else
       {
         return 0
       }
    },
    // padding:function (value,len)
    // {
    //     let sv = String(value)
    //     while(sv.length<len){
    //         sv = '0' + sv
    //     }
    //     return sv
    // },
    reverseMessage: function () {
      // var arr = this.tableData[1].question.split(/\+|-|×|÷/)
      // alert(arr.length)
      //  alert(arr)
      //  var arr = this.tableData[1].question.split(/[^\d/]/)
      // alert(arr.length)
      alert("敬请期待")
    },
    createList:function(){
      //clear cost
      this.start = new Date()

      this.tableData = new Array()
      //setting from UI
      let parentheses = this.getParenthese() 

      //anyway,create 10 question everytime
      let count = 10
      for (let i=0;i<count;i++)
      { 
          let date = {}
          date.question = this.createQ(parentheses)
          date.answer = this.createA(date.question)
          this.tableData.push(date) 
      }
    },
    createQ:function(parentheses){
      
      //init operator
      //1 +  2 -  3 * 4 /
      let operator = new Array()
      let simleft = this.calcNum-2
      let minoperator = 1
      for (let i=0;i<this.calcNum-1;i++)
      { 
          if(simleft == 0)
          {
            minoperator = 3
          }

          let oper = this.rndInt(minoperator,4);

          if(oper<3)
          {
            simleft--
          } 
          operator.push(oper)
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
          let defaultMax = this.getdefaultMax()
          //left parentheses
          if (i<this.calcNum-1)
          {
             if (this.getDefaultW(operator[i])<weight[i])
             {
                calc=calc+"("
                need_parentheses = true
             }
          }
          //min is 3
          let main_num = this.rndInt(3,defaultMax)

          switch(parseInt(this.calcType))
          {
              case 2:
                calc = calc + main_num + "." + this.rndInt(1,defaultMax)
                break;
              case 3: 
                let fraction =reductionofAFraction({numerator:this.rndInt(1,main_num-1),denominator:main_num}
) 
                calc = calc + fraction.numerator + "/" + fraction.denominator
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
        return  Math.floor(Math.random()*(m-n+1)+n)
    },
    getdefaultMax :function (){
        //4:max 20 |5:max 30|6:max 40
        let start = this.calcNum>4?(parseInt(this.calcNum)-3)*10:3
        let end = (parseInt(this.calcNum)-2)*10
        let max = this.rndInt(start,end)
        return max
    },
    getParenthese :function (){
        return parseInt(this.calcNum)-3
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
      var _this = this
      this.timer = setInterval(
        function(){
          _this.cost = _this.countup()
          _this.leftTime = _this.countdown()
        },
      //1)
      1000)
    })
  },
  beforeDestroy: function () {
    if(this.timer){  
       clearInterval(this.timer);
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
