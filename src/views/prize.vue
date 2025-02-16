<!-- <template>

    <div v-for="line,index in tableData" >
      <el-carousel  direction="vertical" :autoplay="false" :motion-blur="true" :loop="true">
    <el-carousel-item v-for="item in pic_list" :key="item" >
     <div class="image-container">
        <el-image style="height: auto;width:{{ widthtble }}" :src="item" alt="logo" :fit="fit" />
        <div class="line horizontal" v-show="line[0].x===1"></div>
          <div class="line vertical"  v-show="line[0].y===1"></div>
          <div class="line diagonal"  v-show="line[0].tl_br===3"></div>
          <div class="line diagonal2"  v-show="line[0].tr_bl===4"></div>
     </div>


    </el-carousel-item>
  </el-carousel>
</div>

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
const value_list=[0,1,2,3,4,5,6,7]
const widthtble=ref(100)
const pic_list=[turtlePic0,turtlePic1,turtlePic2,turtlePic3,turtlePic4,turtlePic5,turtlePic6,turtlePic7]
const carousel10=ref(2)
const carousel11=ref(null)
const carousel12=ref(null)
const tableData=ref([
    [{
        index_n:0,
        X:2,
        y:0,
        tl_br:0,
        tr_bl:0
    },
    {
        index_n:0,
        X:0,
        y:0,
        tl_br:0,
        tr_bl:0
    },
    {
        index_n:0,
        X:0,
        y:0,
        tl_br:0,
        tr_bl:0
    },

],[{
        index_n:0,
        X:0,
        y:0,
        tl_br:0,
        tr_bl:0
    },
    {
        index_n:0,
        X:0,
        y:0,
        tl_br:0,
        tr_bl:0
    },
    {
        index_n:0,
        X:0,
        y:0,
        tl_br:0,
        tr_bl:0
    },

],[{
        index_n:0,
        X:0,
        y:0,
        tl_br:0,
        tr_bl:0
    },
    {
        index_n:0,
        X:0,
        y:0,
        tl_br:0,
        tr_bl:0
    },
    {
        index_n:0,
        X:0,
        y:0,
        tl_br:0,
        tr_bl:0
    },

]])
const getRandomValue = () => {
  const randomIndex = Math.floor(Math.random() * pic_list.length);
  return value_list[randomIndex];
}

let interval1 = setInterval(() => {
    for (let i=0;i<tableData.value.length;i++) {
        let n= getRandomValue()
      tableData.value[i][0].index_n =n
    }

  }, 50);
</script> -->
<template>
<el-carousel  direction="vertical" :autoplay="false" :motion-blur="true" :loop="true" ref="carousel">
    <el-carousel-item v-for="item in pic_list" :key="item" >
     <div class="image-container">
        <el-image style="height: auto;" :src="item" alt="logo" :fit="fit" />
     </div>


    </el-carousel-item>
  </el-carousel>
  </template>

  <script setup>
  import {onMounted, ref, vShow} from "vue";
  import turtlePic0 from  '@/assets/0.png';
import turtlePic1 from  '@/assets/1.png';
import turtlePic2 from  '@/assets/2.png';
import turtlePic3 from  '@/assets/3.png';
import turtlePic4 from  '@/assets/4.png';
import turtlePic5 from  '@/assets/5.png';
import turtlePic6 from  '@/assets/6.png';
import turtlePic7 from  '@/assets/7.png';
const carousel = ref(null);
const getRandomValue = () => {
  const randomIndex = Math.floor(Math.random() * pic_list.length);
  return randomIndex;
}

var n=0
var oldn=0
onMounted(()=>{
   
    let interval1 = setInterval(async () => {
        n=getRandomValue()
        console.log(n)
        if (carousel.value) {
            n=getRandomValue()
            console.log("mod",n%8)
            carousel.value.setActiveItem(n)
            
            //setTimeout(await function(){ carousel.value.next()}, 50);
                
            
            oldn=n
            console.log(n)
        }
       
        

}, 500);
})

const pic_list=[turtlePic0,turtlePic1,turtlePic2,turtlePic3,turtlePic4,turtlePic5,turtlePic6,turtlePic7]

  </script>

<style scoped>
.el-carousel__item h3 {
  color: #475669;
  opacity: 0.75;
  line-height: 200px;
  margin: 0;
  text-align: center;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n + 1) {
  background-color: #d3dce6;
}
</style>
