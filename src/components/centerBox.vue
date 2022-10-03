<template>
  <div class="center">
    <div class="top">
      <div class="info1 info">
        <h3>当前警情数(起)</h3>
        <div class="data">
          <span class="num">67</span>
          <span class="content">
            <span style="color: #fe3e12;">日环比:+3%</span>
            <span style="color:#12fe81">周环比:-2%</span>
          </span>
        </div>
      </div>
      <div class="info2 info">
        <h3>区域拥堵指数</h3>
        <div class="data">
          <span class="num">1.4</span>
          <span class="content">
            <span>缓行</span>
            <span>平均车速120Km/h</span>
          </span>
        </div>
      </div>
      <div class="info3 info">
        <h3>当前警情数(起)</h3>
        <div class="data">
          <span class="num">99</span>
          <span class="content">
            <span style="color: #fe3e12;">日环比:+3%</span>
            <span style="color:#12fe81">周环比:-2%</span>
          </span>
        </div>
      </div>
      <div class="info4 info">
        <h3>当前警情数(起)</h3>
        <div class="data">
          <span class="num">7421</span>
          <span class="content">
            <span style="color:#12fe81">月环比:-2%</span>
          </span>
        </div>
      </div>
    </div>
    <div class="core" id="core">
      <img src="../assets/images/ksh42.png" alt="">
      <img src="../assets/images/ksh43.png" alt="">
      <img src="../assets/images/ksh44.png" alt="">
      <img src="../assets/images/ksh45.png" alt="">
      <div class="title">
        <h2>出行服务+大数据</h2>
        <h3>交通大数据分析平台</h3>
      </div>
      <div class="pic" id="pic"></div>
      <div class="pay">
        <div class="left">
          <h2>今日实时收费</h2>
          <h3>省份数据</h3>
          <div class="province">
            <span v-for="item in city" :key="item">
              {{item}}
            </span>
          </div>
        </div>
        <div class="right">
          <div class="ring" id="ring1"></div>
          <div class="ring" id="ring2"></div>
          <div class="ring" id="ring3"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { Pie } from '@antv/g2plot';
import { onMounted } from 'vue';
import { Flow } from '@antv/l7plot';
import trafficMap from "../assets/data/trafficMap";

const city = ['武汉', '北京', '上海', '郑州', '杭州', '天津', '重庆', '太原', '吉林'];
const data = [
  { type: '分类一', value: 10 },
  { type: '其他', value: 5 },
];
const miniOptions = {
  appendPadding: 10,
  data,
  angleField: 'value',
  colorField: 'type',
  color: ['#57c8f8', 'transparent'],
  radius: 1,
  innerRadius: 0.78,
  label: null,
  interactions: [{ type: 'element-selected' }, { type: 'element-active' }],
  statistic: {
    title: false,
    content: {
      style: {
        fontSize: 15,
        whiteSpace: 'pre-wrap',
        overflow: 'hidden',
        textOverflow: 'ellipsis',
        color: 'white',
        fontWeight: 300
      },
      content: '车辆总数',
    },
  },
  legend: false,
  pieStyle: {
    lineOpacity: 1,
    lineWidth: 0
  }
};
const miniOptions2 = {
  appendPadding: 10,
  data: [
    { type: '分类一', value: 30 },
    { type: '其他', value: 5 },
  ],
  angleField: 'value',
  colorField: 'type',
  color: ['#16d9b3', 'transparent'],
  radius: 1,
  innerRadius: 0.78,
  label: null,
  interactions: [{ type: 'element-selected' }, { type: 'element-active' }],
  statistic: {
    title: false,
    content: {
      style: {
        fontSize: 15,
        whiteSpace: 'pre-wrap',
        overflow: 'hidden',
        textOverflow: 'ellipsis',
        color: 'white',
        fontWeight: 300
      },
      content: '今日上线',
    },
  },
  legend: false,
  pieStyle: {
    lineOpacity: 1,
    lineWidth: 0
  }
};
const miniOptions3 = {
  appendPadding: 10,
  data: [
    { type: '分类一', value: 6 },
    { type: '其他', value: 5 },
  ],
  angleField: 'value',
  colorField: 'type',
  color: ['#de6806', 'transparent'],
  radius: 1,
  innerRadius: 0.78,
  label: null,
  interactions: [{ type: 'element-selected' }, { type: 'element-active' }],
  statistic: {
    title: false,
    content: {
      style: {
        fontSize: 15,
        whiteSpace: 'pre-wrap',
        overflow: 'hidden',
        textOverflow: 'ellipsis',
        color: 'white',
        fontWeight: 300
      },
      content: '今日报警',
    },
  },
  legend: false,
  pieStyle: {
    lineOpacity: 1,
    lineWidth: 0
  }
};
const p3 = () => {
  return new Flow('pic', {
    map: {
      type: 'amap',
      style: 'blank',
      center: [105.3956, 30.9392],
      pitch: 0,
      zoom: 2.3,
    },
    source: {
      data: trafficMap,
      parser: {
        type: 'json',
        x: 'x1',
        y: 'y1',
        x1: 'x',
        y1: 'y',
      },
    },
    autoFit: false,
    shape: 'arc',
    size: 1.5,
    color: {
      field: 'count',
      value: ['rgba(1,124,247,0.9)', 'rgba(230,129,28,0.9)'],
      scale: { type: 'quantize' },
    },
    style: {
      opacity: 0.8,
      segmentNumber: 60,
    },
    animate: {
      interval: 2,
      trailLength: 1,
      duration: 2,
    },
    radiation: {
      color: 'white',
      size: 30,
    },
    label: {
      visible: false,
    },
  });
}
const p4 = () => {
  return new Pie('ring1', miniOptions);
}

const p5 = () => {
  return new Pie('ring2', miniOptions2);
}

const p6 = () => {
  return new Pie('ring3', miniOptions3);
}

onMounted(() => {
  p3();
  p4().render();
  p5().render();
  p6().render();
})
</script>

<style lang="less" scoped>
.center {
  flex: 2;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;

  .top {
    flex: 1;
    display: flex;
    width: 96%;
    margin-top: 13px;

    .info {
      margin-right: 5px;
      background-repeat: no-repeat;
      background-size: 100% 100%;
      position: relative;

      h3 {
        position: relative;
        font-size: 14px;
        font-weight: 400;
        top: 12px;
        left: 15px;
      }
    }

    .info1,
    .info3 {
      background-image: url(@/assets/images/ksh40.png);
      width: 20%;

      .data {
        display: flex;
        justify-content: center;
        align-items: center;

        margin-top: 13px;
        display: flex;
        align-items: center;
        padding-left: 8px;

        .num {
          font-family: 'yjsz';
          color: #20dbfd;
          font-size: 40px;
          text-shadow: 0 0 25px #00d8ff;
        }

        .content {
          display: flex;
          flex-direction: column;
          font-size: 12px;
          scale: (0.8);
        }
      }
    }

    .info2 {
      background-image: url(../assets/images/ksh39.png);
      width: 30%;

      .data {
        display: flex;
        justify-content: center;
        align-items: center;

        margin-top: 13px;
        display: flex;
        align-items: center;
        padding-left: 15px;

        .num {
          font-family: 'yjsz';
          color: #20dbfd;
          font-size: 40px;
          text-shadow: 0 0 25px #00d8ff;
        }

        .content {
          display: flex;
          flex-direction: column;
          font-size: 12px;
          scale: (0.8);
        }
      }
    }

    .info4 {
      background-image: url(../assets/images/ksh39.png);
      width: 30%;

      .data {
        margin-top: 13px;
        display: flex;
        align-items: center;
        padding-left: 13px;

        .num {
          font-family: 'yjsz';
          color: #20dbfd;
          font-size: 40px;
          text-shadow: 0 0 25px #00d8ff;
        }

        .content {
          display: flex;
          flex-direction: column;
          font-size: 12px;
          scale: (0.8);
        }
      }
    }

  }

  .core {
    flex: 7;
    height: calc(100% - 110px);
    width: 97%;
    background-image: url(../assets/images/ksh41.png);
    background-size: 100% 100%;
    position: relative;
    margin: 10px 10px;

    .title {
      position: absolute;
      padding: 10px 10px;

      h2 {
        font-size: 19px;
        font-weight: 400;
      }

      h3 {
        margin-top: 5px;
        font-size: 16px;
        font-weight: 400;
      }
    }

    .pic {
      width: 100%;
      height: 100%;
    }

    .pay {
      position: absolute;
      height: 150px;
      width: 99%;
      background-color: #0b2547;
      bottom: 3px;
      left: 2px;
      border: 1px solid #345f92;
      display: flex;
      align-items: center;

      .left {
        flex: 1;

        h2,
        h3 {
          font-weight: 550;
          font-size: 13px;
          margin-left: 15px;
          padding-top: 8px;
        }

        h3 {
          padding-bottom: 8px;
          font-size: 12px;
          font-weight: 400;
          margin-left: 20px;
        }

        .province {
          flex: 1;
          display: flex;
          flex-wrap: wrap;
          align-items: center;
          justify-content: flex-start;

          span {
            text-align: center;
            font-size: 12px;
            width: calc(100% / 3);
            margin-bottom: 8px;
          }
        }
      }

      .right {
        flex: 1.5;
        display: flex;
        align-items: center;
        height: 90%;

        .ring {
          height: 100%;
          width: calc(100% / 3);
        }
      }

    }

    img {
      position: absolute;
      width: 18px;

      &:nth-child(1) {
        top: 0px;
        left: 0px;
      }

      &:nth-child(2) {
        top: 0px;
        right: 0px;
      }

      &:nth-child(3) {
        bottom: 0px;
        right: 0px;
      }

      &:nth-child(4) {
        bottom: 0px;
        left: 0px;
      }
    }
  }
}
</style>