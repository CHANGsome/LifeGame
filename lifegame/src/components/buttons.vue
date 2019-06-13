<template>
  <div class="footer">
     <select @change="initChange" v-model="selected">
         <option v-for="(item,index) in listValue" v-bind:value="index">{{item}}</option>
     </select>
     <button @click="autoStart($event)" value="0" id="button" class="button">Start</button>
     <button @click="toNextState"  class="button">Next</button>
     <select @change="speedChange" v-model="speedSelect">
         <option v-for="(item,index) in listSpeed" :value="index">{{item}}</option>
     </select>
  </div>
</template>
<script>
  export default {
      name: 'Buttons',
      data: function(){
          return {
              selected:0, //下拉框选中值
              listValue:['Clear','Glider','Small Exploder','Exploder',"Random"],
              speedSelect:1000,
              listSpeed:{
                '2000': 'slow',
                '1000': 'middle',
                '500': 'fast'
              }
          }
      },
      methods:{
        initChange(){
          let data=[];
         switch(this.selected){
            case 0:
              data=[];
              break;
            case 1:
              data=[[0,1],[1,2],[2,0],[2,1],[2,2]];
              break;
            case 2:
              data = [[0,1],[1,0],[1,1],[1,2],[2,0],[2,2],[3,1]]
              break;
            case 3:
              data = [[0,0],[0,2],[0,4],[1,0],[1,4],[2,0],[2,4],[3,0],[3,4],[4,0],[4,2],[4,4]]
              break;
            case 4:
              data="random";
              break;
         }
          this.$emit("initData",data)
      },
      speedChange(){
        var isStart=document.getElementById("button").value;
        if(isStart){
          this.$emit("start",this.speedSelect)
        }
        console.log(this.speedSelect);
      },
      toNextState(){
        this.$emit("next")
      },
      autoStart(){
        var buttonDom=document.getElementById("button");
        if(buttonDom.value==0){  //开始
          buttonDom.value=1;
          buttonDom.innerText="Stop";
          this.$emit("start",this.speedSelect)
        }
        else{  //结束
          buttonDom.value=0;
          buttonDom.innerText="Start";
          //TODO close事件
          this.$emit("end");
        }
        
      }
  }
}
</script>
<style scoped>
.footer{
  position: fixed;
  bottom: 0;
  height: 50px;
  background: #9999;
  width: 100%;
  text-align: center;
  line-height: 50px;
}
select{
  height: 30px;
  font-size: 15px;
}
.button{
  height: 30px;
  width: 70px;
  font-size: 15px;
  letter-spacing: 1px;
}
</style>