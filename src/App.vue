<template>
  <div id="app">
    <input type="text" v-model='userChekced' @click='flag=true' readonly>
    <div class="selectBox" v-if='flag==true'>
        <select  name="" id="" v-model='checkedYear' >
          <option :value="item" v-for='item in yearArr' :selected="item == year">{{item}}</option>
        </select>
        <select name="" id="" v-model='checkedMonth'>
            <option :value="item" v-for='item in 12' :selected='item ==month'>{{item}}</option>
        </select>
        <div class="data">
            <div class="dHead">
              <span v-for='(item,index) in dhead' :class='{ red: index==0 || index ==6}'>{{item}}</span>
            </div>
            <div class="dbody" @click='submit($event)'>
              <span  v-for='(item,index)  in dateArr' :class="{ red: index==0 || index ==6 || index%7==0 || (index+1)%7 == 0, gray: item == checkedDate}">
                {{item}}
              </span>
            </div>
        </div>
    </div>
  
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      now : new Date().toLocaleDateString(),
      dhead:["星期天","星期一","星期二","星期三","星期四","星期五","星期六"],
      year:new Date().getFullYear(),
      month:new Date().getMonth()+1,
      date: new Date().getDate(),
      checkedYear:new Date().getFullYear(),
      checkedMonth: new Date().getMonth()+1,
      checkedDate:0,
      userChekced:'',
      flag:false,
    }
  },
  computed:{
    yearArr(){
      let arr = [];
      let data = this.year-20;
      for(;data<=this.year+20;data++){
        arr.push(data)
      }
      return arr
    },
    dateArr(){
      let data = 0;
      let dateArr = [];
      switch(this.checkedMonth){
        case 1:
        case 3:
        case 5:
        case 7:
        case 8:
        case 10:
        case 12:
         data = 31;
         break;
         case 2:
         if((this.checkedYear)%4 ==0 && this.checkedYear !=0){
           data = 29
         }else{
           data = 28
         }
         break;
         default:data = 30;
         break;
      }
      for(let i=1; i<=data;i++){
        dateArr.push(i) 
      }
      let filters = new Date(`${this.checkedYear}/${this.checkedMonth}/1`).getDay();
      for(let i=0;i<filters;i++){
          dateArr.unshift('')
        }
     

      return dateArr
    },  
  },
  methods:{
    submit(e){
      let target = e.target
      if(target.nodeName == "SPAN" && target.textContent !=""){
          this.checkedDate =target.textContent.replace(/\s*/g,"");
          this.userChekced = `${this.checkedYear}/${this.checkedMonth}/${this.checkedDate}`;
             this.flag = false;
      }
   
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.data{
  width:350px;
  margin:0 auto;
  border:solid red 1px;
  height:300px;
  text-align:left;
}
.data .dHead {
  font-size:0;
}
.data span{
  width:50px;
  text-align:center;
  font-size:12px;
  display:inline-block;
  cursor:pointer;
}
.red{
  color:red
}
.gray{
  background:#e5e5e5
}

</style>
