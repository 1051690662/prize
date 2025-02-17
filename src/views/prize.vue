<template>

  <el-table :data="tableData" :show-header="false" border>

    <el-table-column label="" :width="widthtble"
                     v-for="(column, index) in tableData">
      <template #default="{ row }">
        <div class="image-container">
          <el-image style="height: auto" :src="pic_list[row[index].index_n]" alt="logo" :fit="fit"/>
          <div class="line horizontal" v-show="row[0].x===1"></div>
          <div class="line vertical" v-show="row[0].y===1"></div>
          <div class="line diagonal" v-show="row[0].tl_br===1"></div>
          <div class="line diagonal2" v-show="row[0].tr_bl===1"></div>
        </div>
      </template>
    </el-table-column>


  </el-table>


  <el-button @click="getPrize" type="primary">抽奖</el-button>

</template>

<script setup>
import {onMounted, ref, vShow} from "vue";
import {useTransition} from '@vueuse/core'
import turtlePic0 from '@/assets/0.png';
import turtlePic1 from '@/assets/1.png';
import turtlePic2 from '@/assets/2.png';
import turtlePic3 from '@/assets/3.png';
import turtlePic4 from '@/assets/4.png';
import turtlePic5 from '@/assets/5.png';
import turtlePic6 from '@/assets/6.png';
import turtlePic7 from '@/assets/7.png';


const widthtble = ref(100)
//const pic_list = [turtlePic0, turtlePic1, turtlePic2, turtlePic3, turtlePic4, turtlePic5, turtlePic6, turtlePic7]
const pic_list = [turtlePic0, turtlePic1, ]
const carousel10 = ref(2)
const carousel11 = ref(null)
const carousel12 = ref(null)
const tableData = ref([
  [{
    index_n: 0,
    X: 0,
    y: 0,
    tl_br: 0,
    tr_bl: 0
  },
    {
      index_n: 0,
      X: 0,
      y: 0,
      tl_br: 0,
      tr_bl: 0
    },
    {
      index_n: 0,
      X: 0,
      y: 0,
      tl_br: 0,
      tr_bl: 0
    },

  ], [{
    index_n: 0,
    X: 0,
    y: 0,
    tl_br: 0,
    tr_bl: 0
  },
    {
      index_n: 0,
      X: 0,
      y: 0,
      tl_br: 0,
      tr_bl: 0
    },
    {
      index_n: 0,
      X: 0,
      y: 0,
      tl_br: 0,
      tr_bl: 0
    },

  ], [{
    index_n: 0,
    X: 0,
    y: 0,
    tl_br: 0,
    tr_bl: 0
  },
    {
      index_n: 0,
      X: 0,
      y: 0,
      tl_br: 0,
      tr_bl: 0
    },
    {
      index_n: 0,
      X: 0,
      y: 0,
      tl_br: 0,
      tr_bl: 0
    },

  ]])

let intervals = [];


const getRandomValue = () => {
  return Math.floor(Math.random() * pic_list.length);
}
const updateTableData=(columnIndex,time=50)=> {
  return setInterval(() => {
    for (let i = 0; i < tableData.value.length; i++) {
      tableData.value[i][columnIndex].index_n =getRandomValue();
    }
  }, time);
}

const start=(columnCount=tableData.value.length)=>{
  for (let columnIndex = 0; columnIndex < columnCount; columnIndex++) {
    intervals.push(updateTableData(columnIndex));
  }
}

const stop=async(delay = 1000)=> {

     for (let i = 0; i < intervals.length; i++) {
    await new Promise((resolve) => {
      setTimeout(() => {
        clearInterval(intervals[i]); // 清除定时器
        resolve();
      }, delay * (i + 1)); // 按顺序延迟清除
    });
  }
  intervals = []; // 清空 intervals 数组

}

const checkRow=()=>{
  let data= tableData.value
  for(let i=0;i< data.length;i++){
    console.log( data[i])
   if( data[i][0].index_n === data[i][1].index_n && data[i][1].index_n === data[i][2].index_n){
     tableData.value[i][0].x=1
      tableData.value[i][1].x=1
      tableData.value[i][2].x=1
   }
  }
}
const checkCoL=()=>{
  let data= tableData.value
  for(let i=0;i< data.length;i++){
    console.log( data[i])
   if( data[0][i].index_n === data[1][i].index_n && data[1][i].index_n === data[2][i].index_n){
     tableData.value[0][i].y=1
      tableData.value[1][i].y=1
      tableData.value[2][i].y=1
   }
  }
}
const drawLine=()=>{
  checkRow()
  checkCoL()

}

const getPrize=async ()=>{
  let delay=250
  await start()
  console.log(intervals)
  await stop(delay)
  console.log('stop')
  await drawLine()


}

</script>
<style scoped>
.image-container {
  position: relative;
  display: inline-block;
}
.line {
  position: absolute;
  background-color: red;
}

.horizontal {
  top: 50%;
  left: 0;
  width: 100%;
  height: 2px;
  transform: translateY(-50%);
}

.vertical {
  top: 0;
  left: 50%;
  width: 2px;
  height: 100%;
  transform: translateX(-50%);
}

.diagonal {
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(to bottom right, transparent 49%, red 49%, red 51%, transparent 51%);
}
.diagonal2{
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(to bottom left, transparent 49%, red 49%, red 51%, transparent 51%);
}
</style>