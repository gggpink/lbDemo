<template>
  <el-container>
    <el-main>
      <el-aside width="200px">
        <h2>03星星间链路管理</h2>
        <el-button type="primary" @click="centerDialogVisible">申请北斗星间链路</el-button>
        <!-- messObj对象    parClose弹框中的取消  parDetermine弹框中的取消   -->
       <Dialog :messObj = "messObj" @parClose = "parClose" @parDetermine = "parDetermine"></Dialog>
      </el-aside>
    </el-main>
    <el-footer>Footer</el-footer>
  </el-container>
</template>

<script>
import Dialog from './Dialog' // 公共组件弹框
export default {
  name: "HelloWorld",
  data() {
    return {          
      messObj:{
        clickMsg:false, // true公共弹框显示 false则隐藏
        title:"" // 公共弹框title展示
      },
      newAddTime:[]// 新增时间
    };
  },
  
  components:{
    Dialog
  },
  methods: {
    // 点击改变message 显示公共弹框
    centerDialogVisible(){
      this.messObj.clickMsg = true;
      this.messObj.title = "03星星间链路申请"
    },
    // 点击公共弹框取消 调用改方法 改变message隐藏
    parClose(){
      this.messObj.clickMsg = false;
      console.log(this.messObj)
    },
    // 点击公共弹框确定 调用改方法 改变message隐藏
    parDetermine(data){
      this.newAddTime = data
      console.log(this.newAddTime)
      this.messObj.clickMsg = false;
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.el-header,
.el-footer {
  background-color: #b3c0d1;
  color: #333;
  text-align: center;
}

.el-aside {
  background-color: #d3dce6;
  color: #333;
  text-align: center;
}

.el-main {
  background-color: #e9eef3;
  color: #333;
  text-align: center;
}

body > .el-container {
  margin-bottom: 40px;
}

.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {
  line-height: 260px;
}

.el-container:nth-child(7) .el-aside {
  line-height: 320px;
}
.el-header,
.el-footer {
  background-color: #b3c0d1;
  color: #333;
  text-align: center;
  line-height: 60px;
}

.el-aside {
  background-color: #d3dce6;
  color: #333;
  text-align: center;
}

.el-main {
  background-color: #e9eef3;
  color: #333;
  text-align: center;
}

body > .el-container {
  margin-bottom: 40px;
}
</style>
