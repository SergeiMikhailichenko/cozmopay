<script setup>
import { ref } from 'vue';
import { banks } from '../other/banks.js'
import Step4 from './HelloWorld.vue';

const props = defineProps(['data']);
const emit = defineEmits(['next']);

const formData = ref({
    nationalCode: '',
    phoneNumber: '',
});

const smsStatusText = ref('');
const smsCodeText = ref('');
const phone = ref('');

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
const sendSMS = () => {
    if (phone.value) {
        smsStatusText.value = `تم إرسال رسالة نصية قصيرة بنجاح إلى ${ phone.value }!`
        formData.value.phoneNumber = phone.value.replace(/\D/g, '');
    }
};
const smscode = (event) => {
    let value = event.target.value.replace(/\D/g, ''); // Убираем все нецифровые символы
    if (value.length === 6) {
        smsCodeText.value = `بنجاح!`;
    } else {
        smsCodeText.value = ``;
    }
}

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
const formatPhoneNumber = (event) => {
  let value = event.target.value.replace(/\D/g, ''); // Убираем все нецифровые символы
  if (value.length <= 3) {
    value = value.replace(/(\d{1,3})/, '($1');
  } else if (value.length <= 6) {
    value = value.replace(/(\d{1,3})(\d{1,3})/, '($1) $2');
  } else if (value.length <= 10) {
    value = value.replace(/(\d{1,3})(\d{1,3})(\d{1,4})/, '($1) $2-$3');
  } else {
    value = value.replace(/(\d{1,3})(\d{1,3})(\d{1,4})(\d{1,4})/, '($1) $2-$3-$4');
  }
  event.target.value = value;


  phone.value = value; // Обновляем модель

};

</script>

<template>
    <img class="step_img" src="../assets/images/part3.png" alt="">
    <!-- <p>{{ props }}</p>
    <p>{{ formData }}</p> -->
    <div class="content_title">إعادة التعبئة عن طريق البنك {{ formData.bank_info.bank_name }}</div>
    <div class="content_order">رقم الطلب #000454545</div>
    <div class="input_title">بإدخال الرمز الوطني</div>
    
    <div class="input-wrapper">
      <img src="../assets/images/card.svg" alt="Icon" class="input-icon" style="width: 20px; height: 20px; margin: 0 0 0 8px;"/>
      <input type="text" placeholder="أدخل الرمز" class="custom-input" />
    </div>
    
    <div class="input_title" style="margin-top: 23px;">من أجل الحصول على رقم الحساب الخاص بالتحويل، نحتاج إلى تأكيد معاملتك بالرمز الوارد في الرسالة النصية القصيرة</div>
    <div class="input-wrapper" style="flex-direction: row;">
      <img src="../assets/images/phone-call.svg" alt="Icon" class="input-icon" style="width: 20px; height: 20px; margin: 0 0 0 8px;"/>
      <input type="text"
      placeholder="أدخل رقم الحساب. مسموح فقط في هذا الحقل"
      class="custom-input"
      id="phone"
      @input="formatPhoneNumber"
      maxlength="18"
      />
      <button @click="sendSMS" class="field-button">إرسال الرمز</button>
    </div>
    
    <div class="content_order_sms" style="color: #55A548; height: 20px;">{{ smsStatusText }}</div>
    
    <div class="footer_order">
      <div class="footer_title">أدخل الرمز من الرسالة النصية القصيرة</div>
      <input class="footer_input"
      type="text"
      placeholder="_ _ _ _ _ _"
      maxlength="6"
      @input="smscode"
      >
      <div class="footer_status" style="height: 27px;">{{ smsCodeText }}</div>
    </div>
    

        <!-- <button class="btn1">المزيد</button> -->

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
.content_order {
  font-family: IRANSansX;
  font-size: 17px;
  font-weight: 500;
  line-height: 25.5px;
  text-align: center;
  color: #2E75F7;
  margin-top: 10px;
}
.input_title {
  font-family: IRANSansX;
  font-weight: 400;
  text-align: right;
  color: #3A3A3A;
  margin-bottom: 13px;
}
.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #f9f9f9;
  padding: 8px 12px;
  /* max-width: 300px; */
  direction: rtl;
  flex-direction: row-reverse;
  height: 42px;
}
.custom-input {
  border: none;
  outline: none;
  flex: 1;
  /* font-size: 14px; */
  color: #333;
  background: transparent;
  padding-left: 12px;
  width: 50px;
  height: 40px;
}
.custom-input::placeholder {
  color: #aaa;
}
.field-button {
  background-color: #28a745;
  color: #fff;
  border: none;
  border-radius: 8px;
  padding: 10px 20px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  width: 109px;
  height: 43px;
}
.footer_order {
  margin-top: 45px;
  background-color: #3B445C;
  width: calc(100% + 12px);
  height: 184px;
  top: 19px;
  gap: 0px;
  border-radius: 0px 0px 8px 8px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.footer_title {
  font-family: IRANSansX;
  font-size: 14px;
  font-weight: 400;
  line-height: 21px;
  text-align: center;
  margin-top: 25px;
  color: #fff;
}
.footer_input {
  border: 2px solid #E1E1E1;
  border-radius: 8px;
  padding: 10px 15px;
  font-size: 16px;
  color: #000000;
  font-family: monospace;
  text-align: center;
}
.footer_status {
  font-family: IRANSansX;
  font-size: 14px;
  font-weight: 400;
  line-height: 21px;
  text-align: center;
  margin-top: 13px;
  margin-bottom: 30px;
  color: #55A548;
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
        margin-top: 50px;
        max-width: 270px;
    }
    .input_title {
        font-size: 14px;
        line-height: 21px;
        width: 270px;
        margin-top: 50px;
    }
    .input-wrapper {
        width: 244px;
    }
    .content_order_sms {
        font-family: IRANSansX;
        font-size: 13px;
        font-weight: 600;
        line-height: 19.5px;
        text-align: center;
        max-width: 270px;
        margin-top: 20px;
    }
    .custom-input {
        font-size: 10px;
    }
    .footer_input {
        width: 136px;
        height: 36px;
        margin-top: 13px;
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
        margin-top: 40px;
    }
    .input_title {
        font-size: 14px;
        line-height: 21px;
        width: 560px;
        margin-top: 100px;
    }
    .input-wrapper {
        width: 568px;
    }
    .content_order_sms {
        font-family: IRANSansX;
        font-size: 18px;
        font-weight: 600;
        line-height: 27px;
        text-align: center;
        max-width: 654px;
        margin-top: 10px;
    }
    .custom-input {
        font-size: 14px;
    }
    .footer_input {
        width: 136px;
        height: 36px;
        margin-top: 13px;
    }

}








</style>
