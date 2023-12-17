<template>

  <div class="common-layout">

    <el-container>
      <el-aside width="750px">
        <div class="gap">
          <!-- 嵌入 YouTube 影片 -->
          <iframe
            width="700"
            height="500"
            :src="'https://www.youtube.com/embed/sB3k5ntf8OA'"
            frameborder="0"
            allowfullscreen
          ></iframe>
        </div>
          <!-- 左邊區塊一些按鈕 -->
        <div class="gap">
          <el-button color="#626aef" circle>
            <el-icon><CaretTop /></el-icon>
          </el-button>
          <el-button color="#626aef" circle>
            <el-icon><CaretBottom /></el-icon>
          </el-button>
          <el-button color="#626aef" circle>
            <el-icon><CaretLeft /></el-icon>
          </el-button>
          <el-button color="#626aef" circle>
            <el-icon><CaretRight /></el-icon>
          </el-button>
          <el-button type="info" circle>
            <el-icon><HomeFilled /></el-icon>
          </el-button>
        </div>
        <div class="gap">
          <el-button @click="togglePlay" type="success">開啟攝影機</el-button>
          <el-button @click="stopPlay" type="danger">關閉攝影機</el-button>
          <el-button @click="capture" type="primary">拍照</el-button>
        </div>
      </el-aside>
    <el-container>

        <el-header>
          <div style = "text-align:left;">
             <!-- 右邊區塊按鈕 -->
            <el-button type="primary" round v-if="value == 0" @click="manualClean">手動清潔</el-button>&nbsp;&nbsp;
              Automatic Mode:
            <el-tooltip :content="'Switch value: ' + value" placement="top">
            <el-switch
              v-model="value"
              size="large"
              style="--el-switch-on-color: #13ce66; --el-switch-off-color: #ff4949"
              active-value="100"
              inactive-value="0"
              @change="handleSwitchChange"
            />
            </el-tooltip>
          </div>
        </el-header>

      <!-- Cards卡片區 -->
    <el-main>
      <el-row :gutter="12">
        <el-col v-for="(card, index) in cards" :key="index" :span="4">
          <el-card
            :shadow="hover ? 'hover' : 'never'"
            :class="card.color"
          ></el-card>
        </el-col>
      </el-row>
    </el-main>

    <el-main>
      <el-row :gutter="12">
        <el-col v-for="(card, index) in cards2" :key="index" :span="4">
          <el-card
            :shadow="hover ? 'hover' : 'never'"
            :class="card.color"
          ></el-card>
        </el-col>
      </el-row>
    </el-main>

    <el-main>
      <el-row :gutter="12">
        <el-col v-for="(card, index) in cards3" :key="index" :span="4">
          <el-card
            :shadow="hover ? 'hover' : 'never'"
            :class="card.color"
          ></el-card>
        </el-col>
      </el-row>
    </el-main>

    <el-main>
      <el-row :gutter="12">
        <el-col v-for="(card, index) in cards4" :key="index" :span="4">
          <el-card
            :shadow="hover ? 'hover' : 'never'"
            :class="card.color"
          ></el-card>
        </el-col>
      </el-row>
    </el-main>

        <el-footer>
          <div class="demo-progress" style="display: flex;">
            <!-- 完成率 -->
            <div style="margin-left: 60px;"> <!-- 调整 margin-right 的值 -->
              <el-progress type="circle" stroke-width="30" :percentage="completePercentage" />
              <div style="font-weight: bold;">
                Completion Rate:
              </div>
            </div>
            <!-- 壓力、數量 -->
            <div style="text-align: right; display: flex; flex-direction: column; align-items: center;  margin-left: 120px;">
              <div style="font-weight: bold; margin-bottom: 10px;">Pressure:</div>
              <el-progress :percentage="pressurePercentage" :stroke-width="20" color="#ff6600" striped striped-flow/>

              <div style="font-weight: bold; margin-top: 10px;">Quantity:</div><el-statistic :value="quantityValue"></el-statistic>
              <el-progress :percentage="quantityPercentage" :stroke-width="20"   status="success"  striped striped-flow/>

              <el-col :span="6">
              </el-col>
            </div>
          <!-- 電源區塊 -->
            <div style="display: flex; align-items: right; margin-left: 80px;">
              <div style="margin-top: 40px; margin-left: 40px;">
                <el-icon style="font-size: 60px;">
                  <SwitchButton style="font-size: 60px" />
                </el-icon>
              </div>
              <div style="margin-left: 20px;">
                <el-steps direction="vertical" :active="currentStep">
                  <el-step
                    v-for="(step, index) in steps"
                    :key="index"
                    :title="step.title"
                  />
                </el-steps>
              </div>
            </div>

          </div>
        </el-footer>

      </el-container>
    </el-container>
  </div>

</template>

<script>
import { onMounted,watch, ref } from 'vue';
import { Edit, Picture, UploadFilled } from '@element-plus/icons-vue';
import axios from 'axios';


const value = ref(true)

export default {
  data() {
    return {
      apiData: [],
      value: 0,
      initialLoad: true, // 初始
      cards: [
        { color: 'card-one-color1', active: false },
        { color: 'card-one-color1', active: false },
        { color: 'card-one-color1', active: false },
        { color: 'card-one-color1', active: false },
        { color: 'card-one-color1', active: false },
      ],
      cards2: [
        { color: 'card-one-color2', active: false },
        { color: 'card-one-color2', active: false },
        { color: 'card-one-color2', active: false },
        { color: 'card-one-color2', active: false },
        { color: 'card-one-color2', active: false },
      ],
      cards3: [
        { color: 'card-one-color3', active: false },
        { color: 'card-one-color3', active: false },
        { color: 'card-one-color3', active: false },
        { color: 'card-one-color3', active: false },
        { color: 'card-one-color3', active: false },
      ],
      cards4: [
        { color: 'card-one-color4', active: false },
        { color: 'card-one-color4', active: false },
        { color: 'card-one-color4', active: false },
        { color: 'card-one-color4', active: false },
        { color: 'card-one-color4', active: false },
      ],
      hover: true,
      switchValue: 0,
      initialLoad: true,
      pressurePercentage: 0, //壓力百分比
      completePercentage: 0, //完成率
      quantityPercentage: 0, //數量率
      quantityValue: 0, // 數量
      currentStep: 0, // init power
      steps: [
        { title: '0%' },
        { title: '25%' },
        { title: '50%' },
        { title: '75%' },
        { title: '100%' },
      ],
      apiResponse: {
      data: [], //init
      },
    };
  },


  methods: {

    async manualClean() {
    let simulatedApiResponse = {
    status: "success",
    data: [
      { hex: "D49F68110000000000000000", dec: "292069332" },
      { hex: "B455D6540000000000000000", dec: "1423332788" },
      { hex: "NewHex3", dec: "NewDec1" },
      { hex: "NewHex4", dec: "NewDec2" },
      { hex: "NewHex5", dec: "NewDec3" },
      { hex: "NewHex6", dec: "NewDec4" },
      { hex: "NewHex7", dec: "NewDec5" },
      { hex: "NewHex8", dec: "NewDec2" },
      { hex: "NewHex9", dec: "NewDec3" },
      { hex: "NewHex10", dec: "NewDec4" },
      { hex: "NewHex11", dec: "NewDec5" },
      { hex: "NewHex12", dec: "NewDec5" },
      { hex: "NewHex13", dec: "NewDec4" },
      { hex: "NewHex14", dec: "NewDec5" },
      { hex: "NewHex15", dec: "NewDec5" },
      { hex: "NewHex16", dec: "NewDec5" },
    ],
    message: null,
  };

  try {
    let response = await axios.get('https://kohxllw92b.execute-api.ap-northeast-1.amazonaws.com/PROD/rfid');
    let apiData = simulatedApiResponse.data;
    console.log("apiData:" + apiData);

    let numberOfCardsToChange = Math.min(apiData.length, this.cards.length);
    let remainingCardsToChange = apiData.length - numberOfCardsToChange;

    let numberOfCardsToChangeInCards2 = 0; 
    let numberOfCardsToChangeInCards3 = 0; 
    let numberOfCardsToChangeInCards4 = 0; 


    for (let i = 0; i < numberOfCardsToChange; i++) {
      if (!this.cards[i].active) {
        this.cards[i].active = true;
        this.cards[i].color = 'card-orange-color1'; 
        this.currentStep = 1;
        this.pressurePercentage += 5; 
        this.completePercentage += 5; 
        this.quantityPercentage += 5; 
        this.quantityValue++; 
      }
    }

    if (remainingCardsToChange > 0) {
      //cards2需要變色的卡片
      numberOfCardsToChangeInCards2 = Math.min(remainingCardsToChange, this.cards2.length); 
      for (let i = 0; i < numberOfCardsToChangeInCards2; i++) {
        if (!this.cards2[i].active) {
          this.cards2[i].active = true;
          this.cards2[i].color = 'card-orange-color1'; 
          this.currentStep = 2;
          this.pressurePercentage += 5; 
          this.completePercentage += 5; 
          this.quantityPercentage += 5; 
          this.quantityValue++; 
        }
      }
    }

    //相減後丟給cards3要變色的卡片
    let remainingCardsToChange3 = remainingCardsToChange - numberOfCardsToChangeInCards2;
    if (remainingCardsToChange3 > 0) {
      //cards3需要變色的卡片
      numberOfCardsToChangeInCards3 = Math.min(remainingCardsToChange3, this.cards3.length); 
      for (let i = 0; i < numberOfCardsToChangeInCards3; i++) {
        if (!this.cards3[i].active) {
          this.cards3[i].active = true;
          this.cards3[i].color = 'card-orange-color1'; 
          this.currentStep = 3;
          this.pressurePercentage += 5; 
          this.completePercentage += 5; 
          this.quantityPercentage += 5; 
          this.quantityValue++; 
        }
      }
    }

    //相減後丟給cards4要變色的卡片
    let remainingCardsToChange4 = remainingCardsToChange3 - numberOfCardsToChangeInCards3;
    if (remainingCardsToChange4 > 0) {
      //cards4需要變色的卡片
      numberOfCardsToChangeInCards4 = Math.min(remainingCardsToChange4, this.cards4.length);
      for (let i = 0; i < numberOfCardsToChangeInCards4; i++) {
        if (!this.cards4[i].active) {
          this.cards4[i].active = true;
          this.cards4[i].color = 'card-orange-color1'; 
          this.currentStep = 4;
          this.pressurePercentage += 5; 
          this.completePercentage += 5; 
          this.quantityPercentage += 5; 
          this.quantityValue++; 
        }
      }
    }
  } catch (error) {
    console.error('API Error:', error);
  }
},

  async handleSwitchChange(newValue) {
    console.log('Switch value changed:', newValue);
    if (!this.initialLoad) {
      if (newValue == 100) {
        console.log('test11');
        this.resetCardColors();
        await this.startColorChange(); // 等待 startColorChange 完成
        await this.startColorChange2(); // then startColorChange2
        await this.startColorChange3(); // then startColorChange3
        await this.startColorChange4(); // then startColorChange4
      } else if (newValue == 0) {
        console.log('test22');
        // window.location.reload(); 
        this.resetCardColors();// reset & clean
        this.resetCardColors2();
        this.resetCardColors3();
        this.resetCardColors4();
      }
    } else {
      this.initialLoad = false;
    }
  },

async startColorChange() {
  return new Promise((resolve) => {
  let index = 0; // init 0
  const intervalId = setInterval(() => {
    if (index < this.cards.length) {
      if (!this.cards[index].active) {
        this.cards[index].active = true;
        this.cards[index].color = 'card-orange-color1'; 
        this.currentStep = 1;
      }
      index++;
      this.pressurePercentage += 5; 
      this.completePercentage += 5; 
      this.quantityPercentage += 5; 
      this.quantityValue++; 
    } else {
      clearInterval(intervalId); // 如果所有卡片都變色，清除定時器
      resolve();
    }
  }, 50); // 毫秒間隔
});
},
async startColorChange2() {
  return new Promise((resolve) => {
  let index = 0; 
  const intervalId = setInterval(() => {
    if (index < this.cards2.length) {
      if (!this.cards2[index].active) {
        this.cards2[index].active = true;
        this.cards2[index].color = 'card-orange-color1'; 
        this.currentStep = 2;
      }
      index++; 
      this.pressurePercentage += 5; 
      this.completePercentage += 5; 
      this.quantityPercentage += 5; 
      this.quantityValue++; 
    } else {
      clearInterval(intervalId); 
      resolve();
    }
  }, 50); 
});
},
async startColorChange3() {
  return new Promise((resolve) => {
  let index = 0; 
  const intervalId = setInterval(() => {
    if (index < this.cards3.length) {
      if (!this.cards3[index].active) {
        this.cards3[index].active = true;
        this.cards3[index].color = 'card-orange-color1'; 
        this.currentStep = 3;
      }
      index++; 
      this.pressurePercentage += 5; 
      this.completePercentage += 5; 
      this.quantityPercentage += 5; 
      this.quantityValue++; 
    } else {
      clearInterval(intervalId); 
      resolve();
    }
  }, 50); 
});
},
async startColorChange4() {
  return new Promise((resolve) => {
  let index = 0;
  const intervalId = setInterval(() => {
    if (index < this.cards4.length) {
      if (!this.cards4[index].active) {
        this.cards4[index].active = true;
        this.cards4[index].color = 'card-orange-color1'; 
        this.currentStep = 4;
      }
      index++; 
      this.pressurePercentage += 5; 
      this.completePercentage += 5; 
      this.quantityPercentage += 5; 
      this.quantityValue++; 
    } else {
      clearInterval(intervalId); 
      resolve();
    }
  }, 50); 
});
},
async resetCardColors() {
  for (let index = 0; index < this.cards.length; index++) {
    if (this.cards[index].active) {
      this.cards[index].active = false;
      this.cards[index].color = 'card-original-color'; // 原色
    }
  }
  this.pressurePercentage = 0; //還原0
  this.completePercentage = 0; 
  this.quantityPercentage = 0; 
  this.quantityValue = 0; 
  this.currentStep = 0;
},
async resetCardColors2() {
  for (let index = 0; index < this.cards2.length; index++) {
    if (this.cards2[index].active) {
      this.cards2[index].active = false;
      this.cards2[index].color = 'card-original-color'; 
    }
  }
  this.pressurePercentage = 0; 
  this.completePercentage = 0; 
  this.quantityPercentage = 0; 
  this.quantityValue = 0; 
},
async resetCardColors3() {
  for (let index = 0; index < this.cards3.length; index++) {
    if (this.cards3[index].active) {
      this.cards3[index].active = false;
      this.cards3[index].color = 'card-original-color'; 
    }
  }
  this.pressurePercentage = 0; 
  this.completePercentage = 0; 
  this.quantityPercentage = 0; 
  this.quantityValue = 0; 
},
async resetCardColors4() {
  for (let index = 0; index < this.cards4.length; index++) {
    if (this.cards4[index].active) {
      this.cards4[index].active = false;
      this.cards4[index].color = 'card-original-color'; 
    }
  }
  this.pressurePercentage = 0;
  this.completePercentage = 0;  
  this.quantityPercentage = 0; 
  this.quantityValue = 0; 
},
    
  },

}
</script>

<style scoped>

.card-original-color {
  /* 默認顏色 */
  background-color: #ffffff;
}

.card-orange-color1 {
  /* 橘色 */
  background-color: #ff8800;
}

.card-one-color2 {
  /* 默認顏色 */
  background-color: #ffffff;
}

.card-blue-color2 {
  /* 藍色 */
  background-color: #4c00ff;
}

.orange-card {
  background-color: orange;
}

.blue-card {
  background-color: blue;
}

.header-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px;
  height: 60px;
  background-color: #409EFF; 
  color: #fff; 
}

.text-large {
  font-size: 1.5rem; 
}

.font-600 {
  font-weight: 600; 
}
.api-data-container {
  margin-top: 100px; 
}
.el-main{
  --el-main-padding: 10px;
  margin-top: 20px; 
}
.el-row {
  margin-bottom: 20px;
}
.el-row:last-child {
  margin-bottom: 0;
}
.el-col {
  border-radius: 4px;
}

.grid-content {
  border-radius: 4px;
  min-height: 36px;
}

.el-header {
  --el-header-padding: 10px;
}
.card-one-color {
  background-color: lightgrey;
}

.demo-progress .el-progress--circle {
  margin-right: 15px;
}

.demo-progress .el-progress--line {
  margin-bottom: 15px;
  width: 350px;
}
.gap {
  margin-bottom: 20px; 
}

#app{
  margin-top:auto;
}


</style>



