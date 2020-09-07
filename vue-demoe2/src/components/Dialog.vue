
<template>
  <el-dialog :title="messObj.title" :visible.sync="messObj.clickMsg" width="40%" center>
    <el-radio v-model="radio" label="1">常规申请</el-radio>
    <el-radio v-model="radio" label="2">应急申请</el-radio>
    <div class="time" style="position:relative" v-if="radio==1">
      <div v-for="(i,index) in list" :key="index">
        <div class="block" style="margin:20px">
          <span class="demonstration">日期和事件范围</span>
          <el-date-picker
            v-model="Onevalue1[index]"
            type="datetimerange"
            range-separator="至"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
            :picker-options="pickerOptions"
          ></el-date-picker>
        </div>
      </div>
      <el-button class="time_btnAdd el-icon-circle-plus-outline" circle @click="addTime()"></el-button>
      <el-button v-if="list && list.length>1" class="time_btnReduce el-icon-remove-outline" circle @click="reduceTime()"></el-button>
    </div>
    <div class="block" style="margin:20px" v-if="radio==2">
      <span class="demonstration">日期和事件范围</span>
      <el-date-picker
        v-model="Twovalue1"
        type="datetimerange"
        range-separator="至"
        start-placeholder="开始日期"
        end-placeholder="结束日期"
        :picker-options="pickerOptionTwo"
      ></el-date-picker>
    </div>

    <span slot="footer" class="dialog-footer">
      <el-button @click="cancel()">取 消</el-button>
      <el-button v-if="Onevalue1 && Onevalue1.length>0 || Twovalue1!=''" type="primary" @click="determine()">确 定</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  props: {
    messObj: {
      type: Object,
      default: {},
    },
  },
  //  点击新建时间  子组件应该一个清空已经有时间选择器Onevalue1=[]/Twovalue1=""
  data() {
    let that = this;
    return {
      radio: "1",
      Onevalue1: [],
      Twovalue1:"",
      list: [{}],
      Monday:"",
      Sunday:"",
      ThisDSunday:"",
      pickerOptions: { // 常规日期处理
        // 日期 设置常规日期 下周一到下周日
        disabledDate(time) {
          return time.getTime() < that.Monday || time.getTime()>that.Sunday; // 区间为下周一到下周日
        },
      },
      pickerOptionTwo:{ // =应急日期处理
        // 日期 设置应急日期 到本周的周日
        disabledDate(time) {
        return time.getTime() > that.ThisDSunday || time.getTime()< Date.now(); // 区间为当前日期到本周日
        },
      }
    };
  },
  created() {
    this.TimeOne();
    this.TimeSeven();
    this.TimeThisSunday();
  },
  methods: {
    // 点击取消调用父组件的方法 关闭弹框
    cancel() {
      this.$emit("parClose");
    },
    // 点击确定把选择好的时间传给父组件 调用父组件方法 关闭弹框
    determine() {
      if(this.radio=='1'){
        this.$emit("parDetermine",this.Onevalue1);
      }else{
         this.$emit("parDetermine",this.Twovalue1);
      }
     
    },
    addTime() {
      console.log(this.messObj.title)
      this.list.push({});
    },
    reduceTime() {
      this.Onevalue1.splice(this.Onevalue1.length-1, 1);
      this.list.splice(1, 1);
    },
    // 获取当前日期的下周一
    TimeOne() {
      var nowTemp = new Date();
      var oneDayLong = 24*60*60*1000;
      var c_time = nowTemp.getTime();
      var c_day = (nowTemp.getDay()||7)-7;
      var m_time = c_time - (c_day-1)*oneDayLong;
      var monday = new Date(m_time);
      var m_year = monday.getFullYear();
      var m_month = monday.getMonth()+1;
      var m_date = monday.getDate();
      var nextMondayTime = m_year + '-' + m_month + '-' + m_date; // 下周一日期
      this.Monday = new Date(nextMondayTime);
    },
    // 获取当前日期的下周日
    TimeSeven() {
      var nowTemp = new Date();
      var oneDayLong = 24*60*60*1000;
      var c_time = nowTemp.getTime();
      var c_day = (nowTemp.getDay()||7)-14;
      var m_time = c_time - (c_day-1)*oneDayLong;
      var monday = new Date(m_time);
      var m_year = monday.getFullYear();
      var m_month = monday.getMonth()+1;
      var m_date = monday.getDate();
      var nextMondayTime = m_year + '-' + m_month + '-' + m_date; // 下周一日期
      this.Sunday = new Date(nextMondayTime);
    },
    // 获取本周日的日期
    TimeThisSunday() {
      var nowTemp = new Date();
      var oneDayLong = 24*60*60*1000;
      var c_time = nowTemp.getTime();
      var c_day = (nowTemp.getDay()||7)-6;
      var m_time = c_time - (c_day-1)*oneDayLong;
      var monday = new Date(m_time);
      var m_year = monday.getFullYear();
      var m_month = monday.getMonth()+1;
      var m_date = monday.getDate();
      var nextMondayTime = m_year + '-' + m_month + '-' + m_date; // 本周日日期
      this.ThisDSunday = new Date(nextMondayTime);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.time_btnAdd {
  position: absolute;
  right: 60px;
  top: 0px;
}
.time_btnReduce {
  position: absolute;
  right: 10px;
  top: 0px;
}
</style>