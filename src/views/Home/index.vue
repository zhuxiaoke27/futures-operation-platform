<script setup lang="ts">
import { useRouter } from 'vue-router';

/** Static */
const router = useRouter();
const templates = [
  {
    name: '涨停复盘',
    template: 'LimitReview',
    post: 'https://u.thsi.cn/imgsrc/share/4863015c04d4f4a920650d2553b46e1d.png',
  },
  {
    name: '业绩预增',
    template: 'PerformanceForecast',
    post: 'https://u.thsi.cn/imgsrc/share/d071d657714f1c42f7f46032bf141f36.png',
  },
  {
    name: '简单模板',
    template: 'SimpleTemplate',
    post: 'https://u.thsi.cn/imgsrc/share/d071d657714f1c42f7f46032bf141f36.png',
  },
  {
    name: '今日回顾模板',
    template: 'MorningSessionReview',
    post: 'http://u.thsi.cn/imgsrc/share/91e968bc5bbf41be79974b34737e6428.png',
  },
  {
    name: '海报相框模板',
    template: 'ImageFrame',
    post: 'https://u.thsi.cn/imgsrc/share/d071d657714f1c42f7f46032bf141f36.png'
  }
];

/** Methods */
const handleClick = (template: string) => {
  router.push({
    name: 'About',
    query: {
      template,
    },
  });
};
</script>

<template>
  <div class="home">
    <div class="home-title">自动化运营</div>
    <div class="home-container">
      <div
        class="home-container-template"
        v-for="item in templates"
        :key="item.name"
      >
        <img
          class="home-container-template-post"
          :src="item.post"
          :alt="item.name"
          @click="handleClick(item.template)"
        />
        <div class="home-container-template-name">{{ item.name }}</div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="less">
.home {
  width: 100%;
  height: 100%;
  padding: 32px 48px;
  overflow-y: auto;

  &-title {
    font-family: MiSans-Bold;
    font-size: 50px;
    color: #000;
    letter-spacing: 0;
    text-align: justify;
    line-height: 66px;
    font-weight: 700;
    margin-bottom: 36px;
  }

  &-container {
    width: 100%;
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;

    &-template {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-right: 40px;

      &:last-child {
        margin-right: auto;
      }

      &-post {
        width: 295px;
        height: 520px;
        margin-bottom: 18px;
        object-fit: cover;
        border-radius: 5px;
        box-shadow: 0px 4px 10px 0px rgba(0, 0, 0, 0.1);
        &:hover {
          cursor: pointer;
          transform: scale(1.05);
          transition: all 0.3s ease-in-out;
        }
      }

      &-name {
        font-family: MiSans-Bold;
        font-size: 24px;
        color: #000;
        letter-spacing: 0;
        text-align: justify;
      }
    }
  }
}
</style>
