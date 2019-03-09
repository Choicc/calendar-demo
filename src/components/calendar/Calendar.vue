<template>
  <div class="calendar">
    <div class="calendar-header">
      <ul>
        <li v-for="item of calendarHead" :key="item">{{item}}</li>
      </ul>
    </div>
    <div class="calendar-allItem">
      <div class="calendar-item" v-for="(year,index) in dateData" :key="index">
        <div v-for="(month,index) in year" :key="index">
          <div class="calendar-year-month">{{month.dArr[0].date.substr(0,4)+' 年 '+month.dArr[0].date.substr(4,2)+' 月 '}}</div>
          <ul>
            <template>
              <li v-for="(i,index) in [,,,,,,,]" :key="i" v-if="index <month.day"></li>
            </template>
            <li v-for="(day,index) in month.dArr" :key="index">{{day.date.substr(6,2)}}</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  name: "Calendar",
  // props:['startDate','startDate'],
  data() {
    return {
      calendarHead:['日','一','二','三','四','五','六'],
      dateData: [],
      startDate: "2019-03-09",
      endDate: "2020-03-09"
    };
  },
  beforeMount: function() {
    this.setDateData(this.startDate, this.endDate);
    console.log(this.dateData);
  },
  methods: {
    //往dateData填入数据
    setDateData(sdate, edate) {
      let year1 = parseInt(sdate.substr(0, 4));
      let year2 = edate.substr(0, 4);
      let month1 = sdate.substr(5, 2);
      let month2 = edate.substr(5, 2);   
      for (let i = year1; i <= year2; i++) {
        //第一年
        if (i == year1) {
          this.dateData.push(this.monthData(i, parseInt(month1), 12));
        } else if (i > year1 && i < year2) {
          //中间年
          this.dateData.push(this.monthData(i, 1, 12));
        } else {
          //最后一年
          this.dateData.push(this.monthData(i, 1, parseInt(month2)));
        }
      }
    },
    //返回月历数组
    /**
     * year：年份
     * stm：从哪个年份开始返回
     * edm：到哪个年份介绍（包括此年份）
     */
    monthData(year, stm, edm) {
      let mArr = [];
      for (; stm <= edm; stm++) {
        let newStm = stm;
        if (newStm < 10) {
          newStm = "0" + stm;
        }
        let dateCount = this.getMonthDates(year, stm);
        let dArr = [];
        for (let item = 1; item <= dateCount; item++) {
          let newItem = item;
          if (newItem < 10) {
            newItem = "0" + item;
          }
          dArr.push({ date: year + "" + newStm + "" + newItem });
        }
        let date = year + "/" + newStm + "/" + "01";
        let day = new Date(date).getDay();
        mArr.push({ dArr: dArr, day: day });
      }
      //  console.log(mArr)
      return mArr;
    },
    //获取这个月有多少天
    getMonthDates(year, month) {
      var d = new Date(year, month, 0);
      return d.getDate();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calendar-header {
  width: 100%;
  height: 72px;
  background: #f4f4f4;
  position: fixed;
}
.calendar-header ul {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0 20px;
}
.calendar-header ul li,
.calendar-item ul li {
  display: block;
  width: 14.285%;
  height: 90px;
  line-height: 90px;
}

.calendar-item,
.calendar-item ul {
  width: 100%;
}
.calendar-year-month {
  width: 100%;
  text-align: center;
  margin-top: 20px;
}
.calendar-item ul {
  display: flex;
  flex-wrap: wrap;
  padding: 0 20px;
  box-sizing: border-box;
  border-bottom: 1px solid #ccc
}
.calendar-allItem{
  padding-top: 72px;
}
.calendar ul li:nth-child(7n),.calendar ul li:nth-child(7n+1){
  color: red;
}
.calendar-year-month{
  height: 72px;
  line-height: 72px;
  font-size: 28px;
}
</style>
