<template>
  <div>
    <div class="container">
      <div>
          <img src="@/assets/game.png" class="logo">
          <h1 class="title">Game of Life</h1>
      </div>
      <div class="row" v-for="row in grids">
          <div class="grid" v-for="item in row" v-bind:class="{live: item}"></div>
      </div>
    </div>
    <Buttons @initData="setData" @next="toNextState" @start="autoPlay" @end="stopPlay"></Buttons>
  </div>

</template>
<script>
  import Buttons from '@/components/buttons.vue'
  export default {
      name: 'tableGrid',
      data: function(){
          return {
            grids: [],
            time: ""
          }
      },
      mounted: function(){
          this.initTableGrid(30,80);
      },
      methods: {
        initTableGrid(row, col){  //初始化网格
          var arr=[];
          for(let i=0;i<row;i++){
            arr[i]=[];
            for(let j=0;j<col;j++){
              if(i>2*row/5 && i<3*row/5 && j>2*col/5 && j<3*col/5){
                var randomValue = parseInt(Math.random()*10);
                if(randomValue<2){
                  arr[i][j] = 1;
                }else{
                  arr[i][j] = 0;
                }
              }else{
                arr[i][j] = 0;
              }
            }
          }
          this.grids=arr;       
        },
        setData(data){
          this.setZeorGrids();
          console.log(this.grids);
          var arr = this.deepCopy(this.grids);
          if(typeof(data)=="string"){
              this.initTableGrid(30,80);
              return ;
          }
          if(data.length == 0){
            return ;
          }else{
            for(let i=0;i<data.length;i++){
              var x = data[i][0];
              var y = data[i][1];
              arr[x+15][y+40] = 1;
            }
          }
          this.grids = arr;
        },
        setZeorGrids(){
          var arr=[];
          for(let i=0;i<this.grids.length;i++){
            arr[i]=[];
            for(let j=0;j<this.grids[i].length;j++){
                arr[i][j] = 0;
              }
            }
          this.grids=arr;   
        },
        autoPlay(interval){   //自动进行，停止条件是矩阵为全0或矩阵不再变化
          clearInterval(this.time);
          this.time = setInterval(this.toNextState,interval);
        },
        stopPlay(){
          clearInterval(this.time);
        },
        toNextState(){
          var arr = this.deepCopy(this.grids);
          for(let i=1;i<arr.length-1;i++){
            for(let j=1;j<arr[i].length-1;j++){
              var num = this.getNeighborLive([i,j]);
              if(num<2 || num>3){
                arr[i][j] = 0;
              }else if(num == 3){
                arr[i][j] = 1;
              }
            }
          }
          this.grids = arr;
        },
        //获取存活数量
        getNeighborLive(cell){
          var lifes=0,  //存活数
          x=cell[0],   //当前节点的横坐标
          y=cell[1],   //当前节点的纵坐标
          col=this.grids[0].length,   //数组的列数
          row=this.grids.length;     //数组的行数         
          for(let i=x-1;i<x+2;i++){
            for(let j=y-1;j<y+2;j++){                          
                lifes+=this.grids[i][j];
            }
          }
          return lifes-this.grids[x][y];
        },
        deepCopy(obj) {
          // 只拷贝对象
          if (typeof obj !== 'object') return;
          // 根据obj的类型判断是新建一个数组还是一个对象
          var newObj = obj instanceof Array ? [] : {};
          for (var key in obj) {
            // 遍历obj,并且判断是obj的属性才拷贝
            if (obj.hasOwnProperty(key)) {
              // 判断属性值的类型，如果是对象递归调用深拷贝
              newObj[key] = typeof obj[key] === 'object' ? this.deepCopy(obj[key]) : obj[key];
            }
          }
          return newObj;
        }
      },
      components: { 
        Buttons 
      },
  }
</script>
<style>
  body{
    margin: 0;
    padding: 0;
    background-color: beige;
  }
  .container{
    width: 1300px;
    margin: 0 auto;
  }
  .logo{
    vertical-align: middle;
    margin: -12px 20px 0 0;
    width: 80px;
  }
  .title{
    height: 40px;
    color: #2ca506;
    display: inline-block;
  }
  .row{
    height: 16px;
    display: table;
    border-collapse: collapse;
  }
  .row:last-child{
    border: 1px solid #999;
  }
  .grid{
      width: 15px;
      height: 15px;
      display: table-cell;
      border: 0.5px solid #999;
      transition: background-color .3s ease-in;
      border-bottom: 0;
  }
  .live{
    background-color: #3fd112;
  }
</style>
