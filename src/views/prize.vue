<template>

  <el-row>
    <el-col v-for="(stat, index) in statistics" :key="index" :span="stat.span">
      <el-statistic :title="stat.title" :value="stat.value" />
    </el-col>
  </el-row>

  <el-row :gutter="20" >
    <el-col :span="10">
       <el-table :data="tableData" :show-header="false" border>
    <el-table-column label="" :spn="2"
                     v-for="(row, rowIndex) in tableData"
                     >
    <template #default="{row}">
        <div class="image-container">
          <el-image style="height: auto" :src="pic_list[row[rowIndex].index_n]" alt="logo" :fit="fit"/>
          <div class="line horizontal" v-show="row[rowIndex].x===1"></div>
          <div class="line vertical" v-show="row[rowIndex].y===1"></div>
          <div class="line diagonal" v-show="row[rowIndex].tl_br===1"></div>
          <div class="line diagonal2" v-show="row[rowIndex].tr_bl===1"></div>
        </div>
</template>
    </el-table-column>
  </el-table>
    </el-col>
    <el-col :span="4">
    <h1 >中奖分：</h1>
     <p style="color: red; font-size: 50px;">{{cent}}</p>
      <p style="color: red; font-size: 50px;">{{prize_name}}</p>
<el-button @click="getPrize" type="primary">抽奖</el-button>
    </el-col>

  </el-row>





    <el-row>
      <el-row v-for="(stat, index) in pic_list" :key="index" :span="stat.span">
        <div style="text-align: center">
        <el-image style=" display: inline-block;width:50px; margin: 10px; height: auto" :src="stat" alt="logo" :fit="fit" />
          <el-col>{{ pic_list_num[index]}}</el-col>
          </div>
      </el-row>
</el-row>



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
const pic_list_all = [turtlePic0, turtlePic1, turtlePic2, turtlePic3, turtlePic4, turtlePic5, turtlePic6, turtlePic7]
const pic_list = pic_list_all.slice(0,5)
const prize_name=ref('')
let total_prize_times=0
let last_no_prize_times=0
const pic_list_num=ref([0,0,0,0,0,0,0,0])
const cent=ref(0)
const tableData = ref([
  [{index_n: 0, x: 0, y: 0, tl_br: 0, tr_bl: 0},
    {index_n: 0, x: 0, y: 0, tl_br: 0, tr_bl: 0},
    {index_n: 0, x: 0, y: 0, tl_br: 0, tr_bl: 0
    }],
  [{index_n: 0, x: 0, y: 0, tl_br: 0, tr_bl: 0},
    {index_n: 0, x: 0, y: 0, tl_br: 0, tr_bl: 0},
    {index_n: 0, x: 0, y: 0, tl_br: 0, tr_bl: 0},
  ],
  [{index_n: 0, x: 0, y: 0, tl_br: 0, tr_bl: 0},
    {index_n: 0, x: 0, y: 0, tl_br: 0, tr_bl: 0},
    {index_n: 0, x: 0, y: 0, tl_br: 0, tr_bl: 0},
  ]
    ]
)

let intervals = [];
const statistics = ref([
  { title: 'JP1', value: 20, span: 2 },
  { title: 'JP2', value: 50, span: 2 },
  { title: 'JP3', value: 100, span: 2 },
  ]);

const base_prize_times_rate=5
const getRandomValue = (l=pic_list.length) => {
  return Math.floor(Math.random() *l);
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
function getRandomCoordinates(gridSize, numCoordinates) {
  // 创建一个包含所有可能坐标的数组
  const allCoordinates = [];
  for (let x = 0; x < gridSize; x++) {
    for (let y = 0; y < gridSize; y++) {
      allCoordinates.push([x, y]);
    }
  }

  // 随机选取指定数量的坐标
  const selectedCoordinates = [];
  while (selectedCoordinates.length < numCoordinates) {
    const randomIndex = Math.floor(Math.random() * allCoordinates.length);
    const coordinate = allCoordinates.splice(randomIndex, 1)[0];
    selectedCoordinates.push(coordinate);
  }

  return selectedCoordinates;
}

const probabilities = {
   "all_same": 1,
  "three_lines": 3,
  "two_lines": 6,
    "one_line": 20,
    "no_line": 70
}
const winningPatterns = {
    "row_1": [[0, 0], [0, 1], [0, 2]],
    "row_2": [[1, 0], [1, 1], [1, 2]],
    "row_3": [[2, 0], [2, 1], [2, 2]],
    "col_1": [[0, 0], [1, 0], [2, 0]],
    "col_2": [[0, 1], [1, 1], [2, 1]],
    "col_3": [[0, 2], [1, 2], [2, 2]],
    "diag_main": [[0, 0], [1, 1], [2, 2]],
    "diag_anti": [[0, 2], [1, 1], [2, 0]]
};
const chooseWinningType=()=> {
  let r = Math.floor(Math.random() * 100)

  console.log('r',r)
  let total = 0

  for (let [key, p] of Object.entries(probabilities)) {
    console.log(r,key,p)
    total += p
    if(r<total){
      if (key==='all_same'&&statistics.value[2].value<120){
        return "one_line"
      }
      if (key==='three_lines'&&statistics.value[1].value<60){
        return "one_line"
      }
      if (key==='two_lines'&&statistics.value[0].value<25){
        return "one_line"
      }
      return key
    }
  }
  return "no_line"
}
const hasWinningLine=(grid)=>{
  let line=0
  for (const pattern in winningPatterns) {
        const values = new Set(winningPatterns[pattern].map(([x, y]) => grid[x][y]));
        if (values.size === 1) {
          line+=1

        }
    }
  console.log(line)
    return line;
}

const fill_grid=(grid,filledPositions,symbol1=-1)=>{
   let symbol2
  for (let i = 0; i < 3; i++) {
            for (let j = 0; j < 3; j++) {
                if (!filledPositions.has(`${i},${j}`)) {
                  do {
                        symbol2 = getRandomValue();
                      } while (symbol2 === symbol1);
                    grid[i][j] =symbol2
                }
            }
        }
  return grid
}

const generate_grid=()=>{
    let grid = Array.from({ length: 3 }, () => Array(3).fill(''));
    const winType = chooseWinningType()
  console.log('win',winType)

      //选择任n不重复线条
      //随机一个图案x
      //赋值

      //剩余位置
      //随机除x的图案
      //赋值
      let symbol1=-1
      const filledPositions = new Set();
      const numLines = { "one_line": 1, "two_lines": 2, "three_lines": 3, 'all_same':8,"no_line": 0 }[winType];
      const chosenPatterns = Object.keys(winningPatterns).sort(() => 0.5 - Math.random()).slice(0, numLines);
      console.log('chosenPatterns',chosenPatterns)
      if (chosenPatterns) {
        symbol1 = getRandomValue()
      }
      for (const pattern of chosenPatterns) {
        for (const [x, y] of winningPatterns[pattern]) {
                grid[x][y] = symbol1;
                filledPositions.add(`${x},${y}`);
            }
        }
      grid=fill_grid(grid,filledPositions,symbol1)

      while (winType==='no_line'&&hasWinningLine(grid)!==0){
        console.log('no_line',)
        grid=fill_grid(grid,filledPositions)
      }
      while (winType==='two_lines'&&hasWinningLine(grid)!==2){
        console.log('no_line',)
        grid=fill_grid(grid,filledPositions,symbol1)
      }
      while (winType==='one_line'&&hasWinningLine(grid)!==1){
        console.log('no_line',)
        grid=fill_grid(grid,filledPositions,symbol1)
      }

      while(winType!=='all_same'&&hasWinningLine(grid)===8){
        grid[Math.floor(Math.random() )][Math.floor(Math.random() )]=getRandomValue()
      }


    return grid
}

const stop=async(delay = 500)=> {
  let grid=generate_grid()
  console.log('grid',grid)

     for (let i = 0; i < intervals.length; i++) {
    await new Promise((resolve) => {
      setTimeout(() => {
        clearInterval(intervals[i]); // 清除定时器
        tableData.value[0][i].index_n=grid[0][i]
        tableData.value[1][i].index_n=grid[1][i]
        tableData.value[2][i].index_n=grid[2][i]
        resolve()
      }, delay * (i + 1)); // 按顺序延迟清除


    });
  }
  intervals = []; // 清空 intervals 数组

}

const addItemCent=(max=10)=>{
  for (let i=0;i<tableData.value.length;i++){
    for(let j=0;j<tableData.value[0].length;j++){
      let index=tableData.value[i][j].index_n
       pic_list_num.value[index]+=1
      if (pic_list_num.value[index]>max){
            pic_list_num.value[index]-=max
        }
    }
  }
}

const checkSame=(record_data)=> {
   let data= tableData.value
  for (let i = 0; i < data.length; i++) {
    let res_row_index=checkSingleRow(i,data)
    let res_col_index=checkSingleCol(i,data)
    record_data=recordSame(i,res_row_index,record_data,'row')
    record_data=recordSame(i,res_col_index,record_data,'col')
  }

  return record_data

}

const recordSame=(i,index,record_data,way)=>{
  if(index!==-1){
    record_data[way].push([i,index])
  }
  return record_data

}
const checkSingleRow=(i,data)=>{
  let index_n=-1
   if( data[i][0].index_n === data[i][1].index_n && data[i][1].index_n === data[i][2].index_n){
      tableData.value[i][0].x=1
      tableData.value[i][1].x=1
      tableData.value[i][2].x=1
     index_n=data[i][0].index_n
   }
  return index_n
}

const checkSingleCol=(i,data)=>{
  let index_n=-1
  if( data[0][i].index_n === data[1][i].index_n && data[1][i].index_n === data[2][i].index_n){
     tableData.value[0][i].y=1
      tableData.value[1][i].y=1
      tableData.value[2][i].y=1
    index_n=data[0][i].index_n
   }
   return index_n

}

// const checkRow=()=>{
//   let data= tableData.value
//   for(let i=0;i< data.length;i++){
//     console.log( data[i])
//    if( data[i][0].index_n === data[i][1].index_n && data[i][1].index_n === data[i][2].index_n){
//      tableData.value[i][0].x=1
//       tableData.value[i][1].x=1
//       tableData.value[i][2].x=1
//    }
//   }
// }
// const checkCoL=()=>{
//   let data= tableData.value
//   for(let i=0;i< data.length;i++){
//     console.log( data[i])
//    if( data[0][i].index_n === data[1][i].index_n && data[1][i].index_n === data[2][i].index_n){
//      tableData.value[0][i].y=1
//       tableData.value[1][i].y=1
//       tableData.value[2][i].y=1
//    }
//   }
// }
const checkX=(record_data)=>{
  let data= tableData.value
  let res_x_index=-1
  if (data[0][0].index_n === data[1][1].index_n && data[1][1].index_n === data[2][2].index_n) {
    tableData.value[0][0].tr_bl=1
    tableData.value[1][1].tr_bl=1
    tableData.value[2][2].tr_bl=1
    res_x_index=data[0][0].index_n
    record_data=recordSame(0,res_x_index,record_data,'x')
  }
  if (data[0][2].index_n === data[1][1].index_n && data[1][1].index_n === data[2][0].index_n) {
    tableData.value[0][2].tl_br=1
    tableData.value[1][1].tl_br=1
    tableData.value[2][0].tl_br=1
    res_x_index=data[0][2].index_n
    record_data=recordSame(1,res_x_index,record_data,'x')
  }
  return record_data

}
const init=()=>{
  for(let i=0;i<tableData.value.length;i++){
    for(let j=0;j<tableData.value[i].length;j++){
       tableData.value[i][j].x=0
      tableData.value[i][j].y=0
      tableData.value[i][j].tl_br=0
      tableData.value[i][j].tr_bl=0
    }
  }
  cent.value=0
  prize_name.value=''

}
const drawLine=async ()=>{
  let record_data={col:[],
                row:[],
                x:[]}
  record_data=checkSame(record_data)
  record_data=checkX(record_data)
  return record_data
}
const calPrize=async (record_data)=>{
  let total=0
  for(let item in record_data.row){
    total+=pic_list_num.value[record_data.row[item][1]]
  }
  for(let item in record_data.col){
    total+=pic_list_num.value[record_data.col[item][1]]
  }
  for(let item in record_data.x){
    total+=pic_list_num.value[record_data.x[item][1]]
  }

  return total
}

const bigPrize=()=>{
  total_prize_times+=1
  if(total_prize_times%5===0){
    for(let i=0;i<statistics.value.length;i++){
      statistics.value[i].value+=1
    }


  }
}
const lastPrizeTimes=async (record_data)=>{
  console.log('col',record_data.col,record_data.col.length===0)
  console.log('col',record_data.col,record_data.col.length===0)
  console.log('col',record_data.col,record_data.col.length===0)
  let col_lenth=record_data.col.length
  let row_lenth=record_data.row.length
  let x_lenth=record_data.x.length
  let total_line=col_lenth+row_lenth+x_lenth
  if (total_line===0){
    last_no_prize_times+=1
    if (last_no_prize_times>5){
      probabilities.one_line+=5
    }
    console.log("probabilities",probabilities)
    return
  }
  if (total_line===2){
    cent.value=statistics.value[0].value
    statistics.value[0].value=20
    prize_name.value='jp1!'
  }
  if (x_lenth===2 || total_line===3){
    cent.value=statistics.value[1].value
    statistics.value[1].value=50
    prize_name.value='jp2!'
  }
  if(total_line===8){
    cent.value=statistics.value[2].value
    statistics.value[2].value=100
    prize_name.value='jp3!'
  }
  last_no_prize_times=0
  probabilities.one_line=20

}
const getPrize=async ()=>{
  console.log("连续不中奖",last_no_prize_times)
  let delay=50
  await init()
  await start()
  await stop(delay)
  let record_data=await drawLine()
  await addItemCent()
  cent.value+=await calPrize(record_data)
  await lastPrizeTimes(record_data)
  console.log(record_data)
  await bigPrize()


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