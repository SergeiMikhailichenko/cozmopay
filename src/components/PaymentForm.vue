<script setup>
import { ref } from 'vue'
import Step1 from './Step1.vue'
// import '../style.css'

let currentComponent = ref(Step1);
const currentData = ref({});

const handleNext = (nextComponent, data = {}) => {
  currentComponent.value = nextComponent;
  currentData.value = data;
};
</script>

<template>
    <div class="flex items-center justify-center h-screen bg-blue-500">
    <h1 class="text-white text-3xl font-bold">Hello Tailwind CSS!</h1>
  </div>
    <div class="window_pay" dir="ltr">
        <div class="window_header">
        <div class="icon">
          <img src="../assets/images/support_icon.svg" alt="">
        </div>
        <div class="window_caption">
          <div class="window_title">الشعار</div>
          <img src="../assets/images/hand.svg" alt="">
        </div>
      </div>
      <div class="window_content">
        <component :is="currentComponent" :data="currentData" @next="handleNext" />
      </div>
    </div>
    </template>

<style scoped>
.window_pay {
    position: absolute;
    display: flex;
    flex-direction: column;
    /* top: 20px; */
    left: 50%;
    /* transform: translate(-50%, 0); */
    border: 1px;
    border-color: #E6E7ED; 
    border-radius: 8px;
    padding: 6px;
    background-color: #E6E7ED;
    min-height: 400px;
}
.window_header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 15px;
    border-radius: 5px;
    /* width: 100%; */
    background-image:
    radial-gradient(#2F3A5A, #1E2534);
}
.icon {
    display: flex;
    background-color: #333A54;
    border-radius: 7px;
    align-items: center;
    justify-content: center;
}
.window_caption {
    display: flex;
    align-items: center;
    color: #ffffff;
}
.window_title {
    font-family: IRANSansX;
    font-weight: 400;
    text-align: center;
}
.window_content {
    display: flex;
    flex-direction: column;
    height: 100%;
    width: 100%;
    align-items: center;
}

@media (max-width: 800px) {
  .window_pay {
    width: 310px;
    height: 661px;
    transform: translate(-50%, 0);
    top: 20px;
  }
  .window_header {
    height: 64px;
  }
  .icon {
    width: 35px;
    height: 35px;
  }
  .icon img {
    width: 22px;
    height: auto;
  }
  .window_caption {
    gap: 11px;
  }
  .window_caption img {
    width: 22px;
    height: auto;
  }
  .window_title {
    font-size: 17.6px;
    line-height: 26.4px;
  }
}
@media (min-width: 801px) {
  .window_pay {
    width: 800px;
    height: 800px;
    top: 50%;
    transform: translate(-50%, -50%);
  }
  .window_header {
    height: 75px;
  }
  .icon {
    width: 45px;
    height: 45px;
  }
  .icon img {
    width: 28px;
    height: auto;
  }
  .window_caption {
    gap: 15px;
  }
  .window_caption img {
    width: 28px;
    height: auto;
  }
  .window_title {
    font-size: 22.67px;
    line-height: 34px;
  }
}
</style>
