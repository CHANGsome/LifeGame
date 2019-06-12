<template>
  <div>
    <div class="row" v-for="n in 100">
      <div class="grid" v-for="n in 100"></div>
    </div>
    <!-- <div class="row" v-for="row in grids">
        <div class="grid" v-for="item in row" v-bind:class="{live: item}"></div>
    </div> -->
    <button v-on:click="toNextState">Next</button>
  </div>
</template>
<script>
  import Grid from '@/components/grid.vue'
  export default {
      name: 'tableGrid',
      data: function(){
          return {
            grids: [
              [0,0,0,0,0,0,0,0,0],
              [0,0,0,0,0,0,0,0,0],
              [0,0,0,0,0,0,0,0,0],
              [0,0,0,0,1,0,0,0,0],
              [0,0,0,0,0,1,0,0,0],
              [0,0,0,1,1,1,0,0,0],
              [0,0,0,0,0,0,0,0,0],
              [0,0,0,0,0,0,0,0,0],
              [0,0,0,0,0,0,0,0,0]]
          }
      },
      methods: {
        initTableGrid(row=3, col=3){  //初始化网格
          var arr=[];
          for(let i=0;i<row;i++){
            arr[i]=[];
            for(let j=0;j<col;j++){
              arr[i][j]=parseInt(Math.random()*10)%2;
            }
          }
          this.grids=arr;
        },
        toNextState(){
          var arr = this.deepCopyArr(this.grids);
          for(let i=1;i<arr.length-1;i++){
            for(let j=1;j<arr[i].length-1;j++){
              var num = this.getNeighborNum([i,j]);
              console.log(i+"\t"+j+"\t"+num);
              if(num<2 || num>3){
                arr[i][j] = 0;
              }else if(num == 3){
                arr[i][j] = 1;
              }
            }
          }
          this.grids=arr;
        },
        //获取存活数量
        getNeighborNum(cell){
          var lifes=0,  //存活数
          x=cell[0],   //当前节点的横坐标
          y=cell[1],   //当前节点的纵坐标
          col=this.grids[0].length,   //数组的列数
          row=this.grids.length;     //数组的行数         
          for(let i=x-1;i<x+2;i++){
            // if(i<0||i>=col){
            //     continue;
            // }
            for(let j=y-1;j<y+2;j++){
                // if(j<0||j>=row){
                //     continue;
                // }
                // if(i!=x&&j!=y){  //此节点存在则取其中的值
                //     console.log(this.grids[i][j])
                //     lifes+=this.grids[i][j];
                // }                            
                lifes+=this.grids[i][j];
            }
          }
          return lifes-this.grids[x][y];
        },
        deepCopyArr(obj) {
          // 只拷贝对象
          if (typeof obj !== 'object') return;
          // 根据obj的类型判断是新建一个数组还是一个对象
          var newObj = obj instanceof Array ? [] : {};
          for (var key in obj) {
            // 遍历obj,并且判断是obj的属性才拷贝
            if (obj.hasOwnProperty(key)) {
              // 判断属性值的类型，如果是对象递归调用深拷贝
              newObj[key] = typeof obj[key] === 'object' ? this.deepCopyArr(obj[key]) : obj[key];
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
  body{
    margin: 0;
    padding: 0;
  }
  .row{
    height: 11px;
  }
  .grid{
      width: 10px;
      height: 10px;
      display: inline-block;
      border: 0.5px solid #eee;
      background-color: #555;
  }
  .live{
    background-color: yellow;
  }
</style>
