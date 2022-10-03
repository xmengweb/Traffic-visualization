<template>
  <div class="right">
    <div class="event">
      <span>本月发生事件</span>
      <img src="../assets/images/ksh33.png" alt="">
      <el-carousel class="swiper" indicator-position="none">
        <el-carousel-item>
          <div id="swiper1"></div>
        </el-carousel-item>
        <el-carousel-item>
          <div id="swiper2"></div>
        </el-carousel-item>
      </el-carousel>
    </div>
    <div class="charge">
      <span>收费站收费量</span>
      <img src="../assets/images/ksh33.png" alt="">
      <div class="draw" id="trafficCharge"></div>
    </div>
    <div class="rank">
      <span>车站收费流量</span>
      <img src="../assets/images/ksh33.png" alt="">
      <div id="dynamic"></div>
    </div>
  </div>
</template>

<script setup>
import { Radar, Pie } from '@antv/g2plot';
import { Dot } from '@antv/l7plot';
import { onMounted } from 'vue';
import chargeData from "../assets/data/charge";
import { dataLD, dataLD2, dataDy } from "../assets/data/rightBox.js";

const optionsLD = {
  data: dataLD,
  xField: 'name',
  yField: 'star',
  appendPadding: [0, 10, 0, 10],
  meta: {
    star: {
      alias: '数量',
      min: 0,
      nice: true,
      formatter: (v) => Number(v).toFixed(2),
    },
  },
  xAxis: {
    tickLine: null,
    label: {
      style: {
        fill: 'white'
      }
    }
  },
  yAxis: {
    label: false,
    grid: {
      alternateColor: 'rgba(0, 0, 0, 0.04)',
    },
  },
  // 开启辅助点
  point: {
    size: 2,
  },
  area: {},
  radius: 0.65,
  color: '#f79c18'
}

const colors = [
  'rgba(254,255,198,0.95)',
  'rgba(255,238,149,0.95)',
  'rgba(255,217,99,0.95)',
  'rgba(255,175,43,0.95)',
  'rgba(255,135,24,0.95)',
  'rgba(234,10,0,0.95)',
  'rgba(195,0,0,0.95)',
  'rgba(139,0,0,0.95)',
];

onMounted(() => {
  new Radar('swiper1', optionsLD).render();
  optionsLD.data = dataLD2;
  new Radar('swiper2', optionsLD).render();

  const piePlot = new Pie('dynamic', {
    appendPadding: 10,
    data: dataDy,
    angleField: 'value',
    colorField: 'type',
    radius: 0.9,
    label: {
      type: 'inner',
      offset: '-30%',
      content: ({ percent }) => `${(percent * 100).toFixed(0)}%`,
      style: {
        fontSize: 14,
        textAlign: 'center',
      },
    },
    interactions: [{ type: 'element-active' }],
  });
  piePlot.render();
  setInterval(() => {
    piePlot.changeData(dataDy.map((d) => ({ ...d, value: d.value * Math.random() })));
  }, 1200);


  new Dot('trafficCharge', {
    map: {
      type: 'amap',
      style: 'blank',
      center: [102.601, 38.32],
      zoom: 1.5,
      pitch: 0,
    },
    source: {
      data: chargeData,
      parser: { type: 'geojson' },
    },
    color: {
      field: 'PerCapitaGDP',
      value: ({ PerCapitaGDP }) => {
        const index = Math.min(7, ~~(PerCapitaGDP / 10000));
        return colors[index];
      },
    },
    size: {
      field: 'population',
      value: ({ population }) => population / 80,
    },
    style: {
      opacity: 1,
      strokeWidth: 0,
    },
    state: { active: { color: '#1EA7FD' } },
    tooltip: {
      items: [
        { field: '名称', alias: '名称' },
        { field: 'PerCapitaGDP', alias: '数量' },
      ],
    },
  });
})

</script>

<style lang="less" scoped>
.right {
  height: 100%;
  padding-top: 10px;
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;

  .event,
  .charge,
  .rank {
    height: calc(100% / 3);
    font-size: 12px;

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

  .event {
    :deep(.el-carousel) {
      height: 95%;
      overflow: hidden;

      #swiper1,
      #swiper2 {
        width: 100%;
        height: 70%;
        transform: translateY(10px);
      }
    }
  }

  .rank {
    #dynamic {
      height: 90%;
    }
  }
}
</style>