<template>
  <div class="app">
    <div class="header">
      <!-- <img src="./assets/images/jcdsj_logo.gif" alt=""> -->
      <img src="./assets/images/ksh31.png" alt="">
      <span class="titleText">高速管综合大数据</span>
      <img src="./assets/images/ksh32.png" alt="">
    </div>
    <div class="content">
      <div class="left">
        <div class="total">
          <span>交通流量</span>
          <img src="./assets/images/ksh33.png" alt="">
          <div class="draw" id="trafficFlowid"></div>
        </div>
        <div class="category">
          <span>交通工具流量</span>
          <img src="./assets/images/ksh33.png" alt="">
          <div class="draw" id="trafficToolsFlowid"></div>
        </div>
        <div class="money">
          <span>车站收费流量</span>
          <img src="./assets/images/ksh33.png" alt="">
          <div class="pay">
            <div class="title">
              <div>运输方式</div>
              <div>客运量</div>
              <div>货运量</div>
            </div>
            <template v-for="item in 3" :key="item">
              <div class="main">
                <template v-for="item2 in 4" :key="item2">
                  <img src="./assets/images/ksh34.png" alt="" class="xfk">
                </template>
                <div class="info">
                  <div class="info1">
                    <span>公路运输</span>
                  </div>
                  <div class="info2">
                    <img src="./assets/images/ksh38.png" alt="" class="bk">
                    <span>4347.2万人</span>
                  </div>
                  <div class="info3">
                    <img src="./assets/images/ksh38.png" alt="" class="bk">
                    <span>4347.2万人</span>
                  </div>
                </div>
              </div>
            </template>
          </div>
        </div>
      </div>
      <centerBox />
      <RightBox />
    </div>
  </div>
</template>

<script setup>
import { DualAxes, Area } from '@antv/g2plot';
import { onMounted } from 'vue';
import { trfficFlowBar, trfficFlowLine } from "./assets/data/trfficFlow";
import trafficToolsFlow from "./assets/data/trafficToolsFlow";
import centerBox from "./components/centerBox.vue";
import RightBox from './components/rightBox.vue';

const p1 = () => {
  return new DualAxes('trafficFlowid', {
    data: [trfficFlowBar, trfficFlowLine],
    xField: 'year',
    yField: ['value', 'value2'],
    geometryOptions: [
      {
        geometry: 'column',
        seriesField: 'type',
        isGroup: true,
      },
      {
        geometry: 'line',
        seriesField: 'type2',
        color: ['yellow', '#fd6665'],
      },
    ],
    yAxis: {
      value: {
        grid: {
          line: {
            style: {
              stroke: '#f1f3f5',
            },
          }
        },
      },
      value2: {
        grid: null,
        min: 0
      }
    },
  })
};

const p2 = () => {
  return new Area('trafficToolsFlowid', {
    data: trafficToolsFlow,
    xField: 'Date',
    yField: 'value',
    seriesField: 'name',
    xAxis: {
      range: [0, 1],
      tickCount: 12,
    },
    yAxis: {
      range: [0, 1],
      // max: 100
    },
    areaStyle: () => {
      return {
        fill: 'l(270) 0:#061436 1:#0088d4',
        fillOpacity: 0.1
      };
    },
    smooth: true,
    point: {
      color: 'white',
      size: 1
    },
    line: {
      size: [1.5, 1.5, 1.5]
    },
    isStack: false,
    colorField: 'name', // 部分图表使用 seriesField
    color: ['#0088d4', '#db3233', '#89bd1b'],
  })
}

onMounted(() => {
  p1().render();
  p2().render();
})

</script>

<style lang="less" scoped>
.app {
  width: 100vw;
  height: 99vh;
  background-image: url('./assets/images/data08.png');
  overflow: hidden;

  .header {
    height: 35px;
    width: 100%;
    text-align: center;
    padding-top: 5px;

    // img {
    //   margin-top: 10px;
    //   width: 90%;
    // }

    .titleText {
      font-size: 26px;
      color: #c9f1fa;
      margin: 10px 80px;
      background-image: -webkit-linear-gradient(top, #fdfeff, #baf0fc);
      -webkit-background-clip: text; //以盒子内的文字作为裁剪区域向外裁剪，文字之外的区域都将被裁剪掉。
      -webkit-text-fill-color: transparent; //把文字变透明

    }
  }

  .content {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    color: white;
    height: calc(100% - 40px);

    .left {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding-left: 10px;
      height: 100%;

      .total,
      .category,
      .money {
        height: calc(100% / 3);
        font-size: 15px;

        span {
          position: relative;
          top: 6px;
        }

        img {
          width: 100%;
        }

        .draw {
          height: 88%;
        }
      }

      .money {

        .pay {
          height: 100%;

          .title {
            display: flex;
            justify-content: space-around;

            div {
              display: inline-block;
              font-size: 12px;

              &:nth-child(1) {
                transform: translateX(-7px);
              }

              &:nth-child(2) {
                transform: translateX(-16px);
              }

              &:nth-child(3) {
                transform: translateX(-5px);
              }
            }

            margin: 5px 0;
          }

          .main {
            position: relative;
            width: 100%;
            height: calc((100% - 90px) / 3);
            border: 1px solid #0088d4;
            margin-bottom: 10px;
            background-color: #0d2a45;

            .info {
              width: 100%;
              height: 100%;
              position: relative;
              display: flex;
              flex-direction: row;
              justify-content: space-between;
              align-items: center;

              .info1 {
                height: 100%;
                flex: 0.9;

                span {
                  left: 10%;
                }
              }

              .info2 {
                height: 100%;
                flex: 1;
                padding: 5px 5px;

                span {
                  left: 40%;
                  color: #00ffc6;
                }
              }

              .info3 {
                height: 100%;
                flex: 1;
                padding: 5px 5px;

                span {
                  left: 76%;
                  color: #ffd938;
                }
              }

              span {
                position: absolute;
                top: 38%;
                font-size: 12px;
              }

              .bk {
                height: 100%;
              }
            }

            .xfk {
              width: 5px;
              position: absolute;

              &:nth-child(1) {
                top: -2px;
                left: -2px;
              }

              &:nth-child(2) {
                right: -2px;
                top: -2px;
              }

              &:nth-child(3) {
                bottom: -2px;
                left: -2px;
              }

              &:nth-child(4) {
                bottom: -2px;
                right: -2px;
              }
            }
          }
        }

      }
    }



    .right {
      flex: 1;
    }
  }
}
</style>