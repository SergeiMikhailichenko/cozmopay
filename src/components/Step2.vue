<script setup>
import { ref } from 'vue';
import { banks } from '../other/banks.js'
import Step3 from './Step3.vue';

const props = defineProps(['data']);
const emit = defineEmits(['next']);

const formData = ref({
  accountNumber: '',
  selectedAmount: '',
  
});

if (props.data) {
  Object.assign(formData.value, props.data);
}

const hasError = ref(false);
const hasSelectError = ref(false);

const goToNextStep = () => {
    if (formData.value.accountNumber.length !== 16) {
        hasError.value = true;
    } else {
        hasError.value = false;
    }

    if (!formData.value.selectedAmount) {
        hasSelectError.value = true;
    } else {
        hasSelectError.value = false;
    }

    if (!hasError.value && !hasSelectError.value) {
        emit('next', Step3, formData.value);
    }
};

const getBankLogo = (logo) => {
    try {
        return new URL(`../assets/images/banks/${logo}`, import.meta.url).href;
    } catch (error) {
        console.error('Error loading logo:', error);
        return new URL('../assets/images/banklogo.png', import.meta.url).href; // Путь по умолчанию, если изображение не найдено
    }
};


function formatAccNumber(event) {
    let value = event.target.value.replace(/\D/g, '');
    value = value.match(/.{1,4}/g)?.join(' ') || '';
    event.target.value = value;
    formData.value.accountNumber = value.replace(/\s/g, '');
}
function handlePaste(event) {
    event.preventDefault();
    const pastedData = event.clipboardData.getData('text').replace(/\D/g, '').slice(0, 16);
    const formattedData = pastedData.match(/.{1,4}/g)?.join(' ') || '';
    event.target.value = formattedData;
    formData.value.accountNumber = formattedData.replace(/\s/g, '');
}


</script>

<template>
    <img class="step_img" src="../assets/images/part2.png" alt="">
        <div class="content_title">
            إعادة التعبئة عن طريق البنك {{ props.data.bank_info.bank_name }}
            <img :src="getBankLogo(props.data.bank_info.bank_logo)" alt="" style="height: 35px; width: 60px; margin: 0;">
        </div>
          <!-- <img src="../assets/images/banklogo.png" alt="" style="height: 35px; width: 60px; margin: 0;"> -->
        <div class="input_title">أدخل رقم حسابك المصرفي {{ props.data.bank_info.bank_title }}</div>
        <input class="custom_input"
            type="text"
            placeholder="أدخل رقم الحساب. مسموح فقط في هذا الحقل"
            maxlength="19"
            @input="formatAccNumber"
            @paste="handlePaste"
            :class="{ alert: hasError }"
        >
        <div class="input_title" style="margin-top: 23px;">حدد مبلغ التعبئة. انتبه! لا يمكنك تحويل مبلغ آخر بعد المبلغ المحدد. يجب أن يتوافق المبلغ مع المبلغ المحدد بالضبط.</div>
        <div class="dropdown-wrapper">
          <!-- <label for="amount" class="dropdown-label">حدد مبلغ التعبئة. انتبه! لا يمكنك تحويل مبلغ آخر بعد المبلغ المحدد. يجب أن يتوافق المبلغ مع المبلغ المحدد بالضبط.</label> -->
          <select id="amount" class="dropdown" v-model="formData.selectedAmount" :class="{ alert: hasSelectError }">
            <option value="" disabled selected>حدد المبلغ</option>
            <option value="100">100</option>
            <option value="200">200</option>
            <option value="300">300</option>
          </select>
        </div>
        <button @click="goToNextStep" class="btn1">المزيد</button>
</template>

<style scoped>
.content_title {
    display: flex;
    align-items: center;
    font-family: IRANSansX;
    font-weight: 400;
    text-align: center;
    color: #000;
    direction: rtl;
}
.input_title {
    font-family: IRANSansX;
    font-weight: 400;
    text-align: right;
    color: #3A3A3A;
    direction: rtl;
}
.custom_input {
  border: 2px solid #4CAF50;
  border-radius: 8px;
  padding: 10px 15px;
  font-size: 16px;
  color: #000000;
  font-family: monospace;
  text-align: center;
}
.custom_input::placeholder {
  color: #B0B0B0;
}
.custom_input:focus {
  outline: none;
  box-shadow: 0 0 5px rgba(76, 175, 80, 0.5);
}
/*  */
.dropdown-wrapper {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.dropdown-label {
  font-size: 14px;
  color: #333;
  direction: rtl; /* Для арабского текста */
}

.dropdown {
  border: 2px solid #E1E1E1;
  border-radius: 8px;
  padding: 10px 15px;
  font-size: 16px;
  color: #000000;
  font-family: monospace;
  appearance: none; /* Убирает стрелку браузера */
  direction: rtl; /* Для правильного направления текста */
  background: #fff url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-chevron-down"><polyline points="6 9 12 15 18 9"></polyline></svg>') no-repeat left 10px center;
  background-size: 16px;
  margin-top: 13px;
}

.dropdown:focus {
  outline: 2px solid #007bff;
}
/*  */
.btn1 {
  position: relative;
  height: 53px;
  width: 157px;
  border: 1px;
  background: #55A548;
  border-radius: 8px;
  cursor: pointer;
  color: #FEFEFF;
  font-weight: 600;
  text-align: center;
  font-size: 14px;
  white-space: nowrap;
  font-family: 'IRANSansX';
  font-style: normal;
  line-height: 16px;
  display: block;
}
.alert {
    border: 2px solid red;
    background-color: #ffe6e6;
}

@media (max-width: 800px) {
    .step_img {
        width: 151px;
        height: auto;
        margin-top: 26px;
    }
    .content_title {
        font-size: 20px;
        line-height: 30px;
        margin-top: 30px;
    }
    .input_title {
        font-size: 14px;
        line-height: 21px;
        width: 270px;
        margin-top: 30px;    
    }
    .custom_input {
        width: 270px;
        height: 36px;
        margin-top: 13px;
    }
    .custom_input::placeholder {
        letter-spacing: 3px;
    }
    .dropdown {
        width: 304px;
        height: 60px;
    }
    .btn1 {
        margin: 30px 0 0;
    }
}
@media (min-width: 801px) {
    .step_img {
      width: 303px;
      height: auto;
      margin-top: 100px;
    }
    .content_title {
        font-size: 30px;
        line-height: 45px;
        margin-top: 126px;
    }
    .input_title {
        font-size: 14px;
        line-height: 21px;
        width: 560px;
        margin-top: 40px;
    }
    .custom_input {
        width: 560px;
        height: 36px;
        margin-top: 13px;
    }
    .custom_input::placeholder {
        letter-spacing: 5px;
    }
    .dropdown {
        width: 594px;
        height: 60px;
    }
    .btn1 {
        margin: 60px 0 69px;
    }
}








</style>
