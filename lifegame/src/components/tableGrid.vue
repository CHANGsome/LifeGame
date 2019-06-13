<template>
  <div>
    <div class="row" v-for="row in grids">
        <div class="grid" v-for="item in row" v-bind:class="{live: item}"></div>
    </div>
    <button v-on:click="autoPlay('1000')">Start</button>
    <button v-on:click="toNextState()">Next</button>
  </div>
</template>
<script>
  import Grid from '@/components/grid.vue'
  export default {
      name: 'tableGrid',
      data: function(){
          return {
            grids: []
          }
      },
      mounted: function(){
          this.initTableGrid(45,100);
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
        //判断矩阵是否全为0
        isAllZeroArr(arr){
          var flag = true;
          for(let i=0;i<arr.length;i++){
            for(let j=0;j<arr[i].length;j++){
              if(arr[i][j]!=0){
                flag = false;
                i = arr.length;
                break;
              }
            }
          }
          return flag;
        },
        //判断两个矩阵是否相等
        isEqualArr(arr1, arr2){
          var flag = true;
          for(let i=0;i<arr1.length;i++){
            for(let j=0;j<arr1[i].length;j++){
              if(arr1[i][j]!=arr2[i][j]){
                flag = false;
                i = arr1.length;
                break;
              }
            }
          }
          return flag;
        },
        autoPlay(interval){   //自动进行，停止条件是矩阵为全0或矩阵不再变化
          setInterval(this.toNextState,interval);
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
        Grid 
      },
  }
</script>
<style>
  .row{
    height: 16px;
    display: table;
  }
  .grid{
      width: 15px;
      height: 15px;
      display: table-cell;
      border: 0.5px solid #fff;
      background-color: #999;
  }
  .live{
    background-color: yellow;
  }
</style>
