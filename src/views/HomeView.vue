

<template>
<el-row>
    <el-col v-for="(stat, index) in statistics" :key="index" :span="stat.span">
      <el-statistic :title="stat.title" :value="stat.value" />
    </el-col>
  </el-row>


  <el-table :data="tableData"  :show-header="false" border >

    <el-table-column   label=""  :width="widthtble">
    <template #default="{ row }">
      <div class="image-container">
        <el-image style="height: auto" :src="pic_list[row.pic1]" alt="logo" :fit="fit" />
        <div class="line horizontal" v-show="row.line1===1"></div>
          <div class="line vertical"  v-show="row.line1===2"></div>
          <div class="line diagonal"  v-show="row.line1===3"></div>
          <div class="line diagonal2"  v-show="row.line1===4"></div>
          </div>
    </template>
    </el-table-column>




    

    <el-table-column   label="" :width="widthtble">
      
    <template #default="{ row,index }">

      <div class="image-container">
        <el-image style="height: auto" :src="pic_list[row.pic2]" alt="logo" :fit="fit" />
        <div class="line horizontal" v-show="row.line2===1"></div>
          <div class="line vertical"  v-show="row.line2===2"></div>
          <div class="line diagonal"  v-show="row.line2===3"></div>
          <div class="line diagonal2"  v-show="row.line2===4"></div>
      </div>
    </template>
    </el-table-column>

    <el-table-column   label=""  :width="widthtble">
      <template #default="{ row }">
      <div class="image-container">
        <el-image style="height: auto" :src="pic_list[row.pic3]" alt="logo" :fit="fit" />
        <div class="line horizontal" v-show="row.line3===1"></div>
          <div class="line vertical"  v-show="row.line3===2"></div>
          <div class="line diagonal"  v-show="row.line3===3"></div>
          <div class="line diagonal2"  v-show="row.line3===4"></div>
          </div>
    </template>
    </el-table-column>

  </el-table>

  <el-row>
      <el-row v-for="(stat, index) in pic_list" :key="index" :span="stat.span">
        <div style="text-align: center">
        <el-image style=" display: inline-block;width:50px; margin: 10px; height: auto" :src="stat" alt="logo" :fit="fit" />
          <el-col>{{ pic_list_num[index]}}</el-col>
          </div>
      </el-row>
</el-row>

  <el-button @click="getPrize" type="primary">抽奖</el-button>

</template>

<script setup>
import {onMounted, ref, vShow} from "vue";
import { useTransition } from '@vueuse/core'
import turtlePic0 from  '@/assets/0.png';
import turtlePic1 from  '@/assets/1.png';
import turtlePic2 from  '@/assets/2.png';
import turtlePic3 from  '@/assets/3.png';
import turtlePic4 from  '@/assets/4.png';
import turtlePic5 from  '@/assets/5.png';
import turtlePic6 from  '@/assets/6.png';
import turtlePic7 from  '@/assets/7.png';
import { ElMessage, ElMessageBox } from 'element-plus'
const widthtble=ref(100)
var value_list=[0,1,2,3,4,5,6,7]
var value_list=[0,1]
var pic_list=[turtlePic0,turtlePic1,turtlePic2,turtlePic3,turtlePic4,turtlePic5,turtlePic6,turtlePic7]

var pic_list_num=ref([10,10,10,10,10,10,10,10])
var pic_line_num=ref([0,0,0,0,1,0,0,0])
let show=ref(false)

//const tableData=[{pic:new URL('@/assets/0.png', import.meta.url).href,id:1},]
const tableData=ref([{pic1:0,pic2:1,pic3:2,line1:0,line2:0,line3:0},
{pic1:3,pic2:4,pic3:5,line1:0,line2:0,line3:0,},
{pic1:6,pic2:7,pic3:0,line1:0,line2:0,line3:0,},
])


const getRandomValue = () => {
  const randomIndex = Math.floor(Math.random() * value_list.length);
  return value_list[randomIndex];
}


const getPrize=async ()=>{
  tableData.value[0].line1=0
  tableData.value[0].line2=0
  tableData.value[0].line3=0
  tableData.value[1].line1=0
  tableData.value[1].line2=0
  tableData.value[1].line3=0
  tableData.value[2].line1=0
  tableData.value[2].line2=0
  tableData.value[2].line3=0

  let interval1 = setInterval(() => {
    for (let i=0;i<tableData.value.length;i++) {
      tableData.value[i].pic1 = getRandomValue()
    }

  }, 50);
  let interval2 = setInterval(() => {
    for (let i=0;i<tableData.value.length;i++) {
      tableData.value[i].pic2 = getRandomValue()
    }

  }, 50);
  let interval3 = setInterval(() => {
    for (let i=0;i<tableData.value.length;i++) {
      tableData.value[i].pic3 = getRandomValue()
    }

  }, 50);

  setTimeout(() => {
  clearInterval(interval1);
}, 600)
   setTimeout(() => {
  clearInterval(interval2);
}, 1100)
   setTimeout(() => {
  clearInterval(interval3);
  dealResult()

}, 1600)


}

const outputValue=(data,duration=500)=>{
  return useTransition(data, {
  duration: duration,
})
}
const jp1=ref(50)
const jp2=ref(100)
const jp3=ref(200)
const all=ref(300)
const statistics = ref([
  { title: 'JP1', value: outputValue(jp1), span: 5 },
  { title: 'JP2', value: outputValue(jp2), span: 5 },
  { title: 'JP3', value: outputValue(jp3), span: 5 },
  { title: 'ALL', value: outputValue(all), span: 5 },
  ]);

const checkRowSame=(data,i)=>{
  let i_list=[]
  console.log('row',data[i].pic1,data[i].pic2,data[i].pic3)
  if (data[i].pic1 === data[i].pic2 && data[i].pic2 === data[i].pic3) {
      i_list.push(data[i].pic1)
      tableData.value[i].line1=1
      tableData.value[i].line2=1
      tableData.value[i].line3=1
      //setTimeout(() =>console.log(`横线中奖`), 500)

    }
  console.log("singlerow",i_list)
  return i_list
}

const checkColSame=(data,i)=>{
  let i_list=[]
  console.log("col",`pic${i+1}`,`pic${i+1}`,`pic${i+1}`)
  if (data[0][`pic${i+1}`]=== data[1][`pic${i + 1}`] && data[1][`pic${i + 1}`] === data[2][`pic${i + 1}`]) {
      i_list.push(data[0][`pic${i+1}`])
      tableData.value[0][`line${i+1}`]=2
      tableData.value[1][`line${i+1}`]=2
      tableData.value[2][`line${i+1}`]=2
    }
  console.log("singlecol",i_list)
  return i_list
}
const checkXSame=(data)=>{
  let i_list=[]
  if (data[0].pic1 === data[1].pic2 && data[1].pic2 === data[2].pic3) {
    i_list.push(data[0].pic1)

  }
  if (data[0].pic3 === data[1].pic2 && data[1].pic2 === data[2].pic1) {
    i_list.push(data[0].pic3)
  }
  return i_list
}

const dealMaxNum=(data)=>{
  for(let i=0;i<data.length;i++){
    let  pic1num=pic_list_num.value[data[i].pic1]
    let   pic2num=pic_list_num.value[data[i].pic2]
      let pic3num=pic_list_num.value[data[i].pic3]
          if (pic1num>100){
            pic_list_num.value[data[i].pic1]=100
          }
          if (pic2num>100){
            pic_list_num.value[data[i].pic2]=100
          }
          if (pic3num>100){
            pic_list_num.value[data[i].pic3]=100
          }
  }
}
const getPicNumber=(data)=>{
  for(let i=0;i<data.length;i++){
    console.log('转盘数',data[i])
    pic_list_num.value[data[i].pic1]+=1
    pic_list_num.value[data[i].pic2]+=1
    pic_list_num.value[data[i].pic3]+=1

  }
}

const dealResult=()=>{
  jp1.value+=1
  jp2.value+=1
  jp3.value+=1
  all.value+=1
  getPicNumber(tableData.value)
  let row_list=[]
  let col_list=[]
  let x_list=[]
  console.log("lenth",tableData.value.length)
  for (let i = 0; i < tableData.value.length; i++) {
    console.log('i',i)
    row_list=[...row_list,...checkRowSame(tableData.value,i)]
    console.log("row_list",row_list)
    col_list=[...col_list,...checkColSame(tableData.value,i)]
  }
  x_list=[...x_list,...checkXSame(tableData.value)]
  let all_i_list=[...row_list,...col_list,]
  all_i_list=[...all_i_list,...x_list]
  console.log("all_i_list",all_i_list)

  for (let index in all_i_list){
    console.log('index',index)
    //pic_list_num.value[all_i_list[index]]+=10
    let truindex=all_i_list[index]
  //   ElMessageBox.alert(`分数:${pic_list_num.value[all_i_list[index]]}` ,'中奖！' , {
  //   // if you want to disable its autofocus
  //   // autofocus: false,
  //   confirmButtonText: 'OK',
  //     callback: (action) => {
  //     pic_list_num.value[all_i_list[index]]=0
  //     },
  //     dangerouslyUseHTMLString: true,


  // })
    console.log("pic_list_num",pic_list_num.value[index])
  }
  dealMaxNum(tableData.value)
}
</script>

<style scoped>
.el-table .warning-row {
  --el-table-tr-bg-color: var(--el-color-warning-light-9);
}
.el-table .success-row {
  --el-table-tr-bg-color: var(--el-color-success-light-9);
}


.image-container {
  position: relative;
  display: inline-block;
}

.table-image {
  width: 100px;
  height: 100px;
  display: block;
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
