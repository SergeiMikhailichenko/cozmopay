<script setup>
import { ref } from 'vue';
import TwStep3 from './TwStep3.vue';
import { banks } from '../other/banks.js'

const props = defineProps(['data']);
const emit = defineEmits(['next']);

// Для слайдера и поля ввода денег
const minValue = 50000;
const maxValue = 10000000;
const inputStep = 1000;

// Ошибки ввода
const hasErrorCurrency = ref(false);
const hasErrorAccount = ref(false);
const hasErrorPhone = ref(false);

const smsCodeNumber = ref('');

const formData = ref({
    accountNumber: '',
    phoneNumber: '',
    amount: minValue,
});    

if (props.data) {
  Object.assign(formData.value, props.data);
}

const getBankLogo = (logo) => {
    try {
        return logo 
        ? new URL(`../assets/images/banks/${logo}`, import.meta.url).href
        : '';
    } catch (error) {
        console.error('Error loading logo:', error);
        return new URL('../assets/images/banks/shetab.svg', import.meta.url).href;
    }
};

// обработка поля лицевого счета
function formatAccountNumber(event) {
    let value = event.target.value.replace(/\D/g, '');
    event.target.value = value;
    formData.value.accountNumber = value.replace(/\s/g, '');
    if(formData.value.accountNumber.length < 16) {
        hasErrorAccount.value = false;
    }
}

function handlePasteAccountNumber(event) {
    event.preventDefault();
    const pastedData = event.clipboardData.getData('text').replace(/\D/g, '').slice(0, 16);
    event.target.value = pastedData;
    formData.value.accountNumber = pastedData.replace(/\s/g, '');
    if(formData.value.accountNumber.length < 16) {
        hasErrorAccount.value = false;
    }
}

function checkAccountNumber() {
    if(formData.value.accountNumber.length < 16) {
        hasErrorAccount.value = true;
    } else {
        hasErrorAccount.value = false;
    }
}

// обработка поля количества

function handleInputAmount(event) {
    hasErrorCurrency.value = false;
    const value = Number(event.target.value);
    event.target.value = value;
}

function handlePasteAmount(event) {
    const value = Number(event.target.value);
    hasErrorCurrency.value = false;
    event.target.value = value;
}
function checkAmount(event) {
    const value = Number(event.target.value);
    if (value < minValue) {
        formData.value.amount = minValue;
        event.target.value = minValue;
    } else if (value > maxValue) {
        formData.value.amount = maxValue;
        event.target.value = maxValue;
    } else {
        hasErrorCurrency.value = false;
        formData.value.amount = Math.floor(value / inputStep) * inputStep;
    }

}

// обработка поля слайдера
const handleRangeChange = (event) => {
    const value = Number(event.target.value);
    hasErrorCurrency.value = false;
    formData.value.amount = value;
};


// обработка поля телефона
const formatPhoneNumber = (event) => {
    let value = event.target.value.replace(/\D/g, '');

    if (value.length > 10) {
        value = value.slice(0, 10);
    }
    if (value.length <= 3) {
        value = value.replace(/(\d{1,3})/, '$1');
    } else if (value.length <= 6) {
        value = value.replace(/(\d{3})(\d{1,3})/, '$1-$2');
    } else if (value.length <= 8) {
        value = value.replace(/(\d{3})(\d{3})(\d{1,2})/, '$1-$2-$3');
    } else {
        value = value.replace(/(\d{3})(\d{3})(\d{2})(\d{1,2})/, '$1-$2-$3-$4');
    }

    event.target.value = value;
    // formData.value.phoneNumber = value;

    if (value.replace(/\D/g, '').length === 10) { 
        formData.value.phoneNumber = '0' + value.replace(/\D/g, ''); // Добавляем код страны, если номер полный
        console.log(formData.value.phoneNumber);
    } else {
         formData.value.phoneNumber = '';
    }









};

function checkPhoneNumber() {
    if(formData.value.phoneNumber.length < 10) {
        hasErrorPhone.value = true;
    } else {
        hasErrorPhone.value = false;
    }
}

// Обработка поля смс
function formatCodeNumber(event) {
    let value = event.target.value.replace(/\D/g, '');
    event.target.value = value;
    smsCodeNumber.value = value.replace(/\s/g, '');
}



const goToNextStep = () => {
    if (formData.value.accountNumber.length === 16 && formData.value.phoneNumber.replace(/\D/g, '').length === 11 && formData.value.amount ) {
        emit('next', TwStep3, formData.value);
    } 

    // Добавить проверку каждого элемента и выводить ошибками


    // else {
    //         hasError.value = true;
    //     }
    // } else {
    //     hasError.value = true;
    // }
        
};

</script>

<template>
    <form action="#" class="w-full rounded-lg border border-gray-200 bg-white p-4 shadow-sm sm:p-6 lg:max-w-xl lg:p-8">
          <!-- Div for input elements styled as a grid -->
          <div class="mb-6 grid grid-cols-1 gap-4">
            <!-- Div for steps in the transaction process -->
            <div>
              <h2 class="sr-only">Steps</h2>
              <!-- Horizontal layout for step indicators -->
              <div class="relative after:absolute after:inset-x-0 after:top-1/2 after:block after:h-0.5 after:-translate-y-1/2 after:rounded-lg after:bg-gray-100">
                <ol class="relative z-10 flex justify-between text-sm font-medium text-gray-500">
                  <li class="flex items-center gap-2 bg-white p-2">
                    <span class="size-6 rounded-full bg-green-600 text-white text-center text-[12px]/6 font-bold"> 1 </span>
                    <span class="hidden sm:block"> کارت بانکی </span>
                  </li>
                  <li class="flex items-center gap-2 bg-white p-2">
                    <span class="size-6 rounded-full bg-green-600 text-white text-center text-[12px]/6 font-bold "> 2 </span>
                    <span class="hidden sm:block"> شماره حساب </span>
                  </li>
                  <li class="flex items-center gap-2 bg-white p-2">
                    <span class="size-6 rounded-full bg-gray-100 text-center text-[10px]/6 font-bold"> 3 </span>
                    <span class="hidden sm:block"> واریز </span>
                  </li>
                </ol>
              </div>
            </div>

            <!-- Account number input area -->
            <label for="card-number-input" class="mt-5 block text-sm font-medium text-gray-900"> لطفا شماره حساب تان در بانک {****} وارد نماید</label>
            <div class="relative">
              <input type="text" 
              id="card-number-input" 
              class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:outline-none focus:border-green-500 block w-full pe-10 p-2.5 text-center" 
              placeholder="************"
              @input="formatAccountNumber"
              @paste="handlePasteAccountNumber"
              @blur="checkAccountNumber"
              :class="{ alert: hasErrorAccount }"
              maxlength="16"
              required />
              <div class="absolute inset-y-0 end-0 top-0 flex items-center pe-3.5 pointer-events-none">
                <img class="h-8 w-auto" :src="getBankLogo(formData.bank_info.bank_logo)" alt="" />
              </div>
            </div>

            <!-- Input for money amount -->
            <div class="relative pt-5">
              <label for="card-number-input" class="mb-4 block text-sm font-medium text-gray-900"> لطفا مبلغ انتخاب یا وارد نماید</label>
              <div class="flex mb-4">
                <button id="dropdown-currency-button" data-dropdown-toggle="dropdown-currency" class="flex-shrink-0 z-10 inline-flex items-center py-2.5 px-4 text-sm font-medium text-center text-gray-900 bg-gray-100 border border-gray-300 rounded-s-lg hover:bg-gray-200 focus:ring-4 focus:outline-none focus:ring-gray-100" type="button">
                  <!-- SVG Placeholder for currency icon -->
                  <svg class="me-1" width="16px" height="16px" viewBox="0 0 64 64" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" aria-hidden="true" role="img" preserveAspectRatio="xMidYMid meet" fill="#000000">
                    <!-- SVG paths omitted for brevity -->
                    <g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"> <path d="M32 2C18.9 2 7.8 10.3 3.7 22h56.6C56.2 10.3 45.1 2 32 2z" fill="#83bf4f"> </path> <path d="M32 62c13.1 0 24.2-8.3 28.3-20H3.7C7.8 53.7 18.9 62 32 62z" fill="#ed4c5c"> </path> <path d="M3.7 22C2.6 25.1 2 28.5 2 32s.6 6.9 1.7 10h56.6c1.1-3.1 1.7-6.5 1.7-10s-.6-6.9-1.7-10H3.7z" fill="#f9f9f9"> </path> <g fill="#ffffff"> <path d="M59.4 44.3l-.3.6h.3v-.6"> </path> <path d="M4.9 44.9l-.3-.6v.6h.3"> </path> <path d="M5.6 18.8h-.5c-.1.1-.2.2-.2.3h.7v-.3"> </path> <path d="M5.6 21v-.3h-1v-.6h1.5v.9h2.2v-2.3H8v1.9h-.6v-1.9h-.3v1.9h-.6v-1.9h-.3v1H4.6c-.1.2-.2.5-.3.7v.6h1.3"> </path> <path d="M5.6 44.9h-.7c0 .1.1.2.2.3h.6l-.1-.3"> </path> <path d="M4.6 44.3h1.5v.9h2.2v-2.3H8v1.9h-.6v-1.9h-.3v1.9h-.6v-1.9h-.3v1H4.5c0 .2.1.3.1.4"> </path> <path d="M5.6 43H4.1c0 .1.1.2.1.3h1.4V43"> </path> <path d="M13.6 21H15v-.3h-1v-.6h1.5v.9h2.2v-2.3h-.3v2h-.6v-2h-.4v2h-.6v-2h-.3v1h-1.9z"> </path> <path d="M13 21v-1.3h-1.3v.4h1v.6h-1.5v-1H9v1.4h.3v-1h1.5v.9z"> </path> <path d="M9.8 20.7h.5v.3h-.5z"> </path> <path d="M8.9 18.8H15v.3H8.9z"> </path> <path d="M11.7 44.3h1v.6h-1.5v-1H9v1.4h.3v-1h1.5v.9H13v-1.3h-1.3z"> </path> <path d="M9.8 44.9h.5v.3h-.5z"> </path> <path d="M8.9 43H15v.3H8.9z"> </path> <path d="M17.4 44.9h-.6v-2h-.4v2h-.6v-2h-.3v1h-1.9v1.3H15v-.3h-1v-.6h1.5v.9h2.2v-2.3h-.3z"> </path> <path d="M23 21h1.3v-.3h-1v-.6h1.5v.9H27v-2.3h-.3v2h-.6v-2h-.3v2h-.6v-2h-.4v1H23z"> </path> <path d="M22.4 21v-1.3H21v.4h1v.6h-1.5v-1h-2.2v1.4h.3v-1h1.6v.9z"> </path> <path d="M18.2 18.8h6.1v.3h-6.1z"> </path> <path d="M19.2 20.7h.5v.3h-.5z"> </path> <path d="M26.7 44.9h-.6v-2h-.3v2h-.6v-2h-.4v1H23v1.3h1.3v-.3h-1v-.6h1.5v.9H27v-2.3h-.3z"> </path> <path d="M19.2 44.9h.5v.3h-.5z"> </path> <path d="M18.2 43h6.1v.3h-6.1z"> </path> <path d="M21 44.3h1v.6h-1.5v-1h-2.2v1.4h.3v-1h1.6v.9h2.2v-1.3H21z"> </path> <path d="M31.7 21v-1.3h-1.3v.4h1v.6h-1.6v-1h-2.2v1.4h.4v-1h1.5v.9z"> </path> <path d="M27.6 18.8h6.1v.3h-6.1z"> </path> <path d="M28.5 20.7h.5v.3h-.5z"> </path> <path d="M32.3 21h1.3v-.3h-1v-.6h1.6v.9h2.2v-2.3H36v2h-.6v-2h-.3v2h-.6v-2h-.3v1h-1.9z"> </path> <path d="M27.6 43h6.1v.3h-6.1z"> </path> <path d="M30.4 44.3h1v.6h-1.6v-1h-2.2v1.4h.4v-1h1.5v.9h2.2v-1.3h-1.3z"> </path> <path d="M28.5 44.9h.5v.3h-.5z"> </path> <path d="M36 44.9h-.6v-2h-.3v2h-.6v-2h-.3v1h-1.9v1.3h1.3v-.3h-1v-.6h1.6v.9h2.2v-2.3H36z"> </path> <path d="M37.8 20.7h.5v.3h-.5z"> </path> <path d="M36.9 18.8H43v.3h-6.1z"> </path> <path d="M41.6 21H43v-.3h-1v-.6h1.5v.9h2.2v-2.3h-.3v2h-.6v-2h-.4v2h-.6v-2h-.3v1h-1.9z"> </path> <path d="M41 21v-1.3h-1.3v.4h1v.6h-1.5v-1H37v1.4h.3v-1h1.5v.9z"> </path> <path d="M36.9 43H43v.3h-6.1z"> </path> <path d="M45.4 44.9h-.6v-2h-.4v2h-.6v-2h-.3v1h-1.9v1.3H43v-.3h-1v-.6h1.5v.9h2.2v-2.3h-.3z"> </path> <path d="M39.7 44.3h1v.6h-1.5v-1H37v1.4h.3v-1h1.5v.9H41v-1.3h-1.3z"> </path> <path d="M37.8 44.9h.5v.3h-.5z"> </path> <path d="M50.4 21v-1.3H49v.4h1v.6h-1.5v-1h-2.2v1.4h.3v-1h1.6v.9z"> </path> <path d="M47.2 20.7h.5v.3h-.5z"> </path> <path d="M46.2 18.8h6.1v.3h-6.1z"> </path> <path d="M51 21h1.3v-.3h-1v-.6h1.5v.9H55v-2.3h-.3v2h-.6v-2h-.3v2h-.6v-2h-.4v1H51z"> </path> <path d="M47.2 44.9h.5v.3h-.5z"> </path> <path d="M49 44.3h1v.6h-1.5v-1h-2.2v1.4h.3v-1h1.6v.9h2.2v-1.3H49z"> </path> <path d="M46.2 43h6.1v.3h-6.1z"> </path> <path d="M54.7 44.9h-.6v-2h-.3v2h-.6v-2h-.4v1H51v1.3h1.3v-.3h-1v-.6h1.5v.9H55v-2.3h-.3z"> </path> <path d="M55.6 19.1h3.5c-.1-.1-.1-.2-.2-.3h-3.4c.1 0 .1.3.1.3"> </path> <path d="M58.4 20.1h1v.6h-1.5v-.9h-2.2v1.3h.3v-1h1.5v.9h2.2v-.5c-.1-.2-.2-.5-.3-.7h-1v.3"> </path> <path d="M56.5 20.7h.5v.3h-.5z"> </path> <path d="M56.5 44.9h.5v.3h-.5z"> </path> <path d="M55.6 43v.3h4.2c0-.1.1-.2.1-.3h-4.3"> </path> <path d="M58.4 44.3h1c.1-.1.1-.2.2-.4h-1.2v.4"> </path> <path d="M57.8 44.9V44h-2.2v1.3h.4v-1h1.5v.9h1.4c.1-.1.1-.2.2-.3h-1.3"> </path> </g> <g fill="#ed4c5c"> <path d="M36.5 33.9c.9-2.6 0-5.5-2-7.3c2.3 3.4 1.4 7.9-1.8 10.3l.2-5.6v-4.4c-.4-.2-.7-.4-.9-.7c-.2.3-.5.6-.9.7v4.4l.2 5.6c-3.3-2.4-4.1-6.9-1.8-10.3c-2.1 1.8-2.9 4.7-2 7.3c.6 1.8 1.8 3.1 3.3 3.9c-.9.3-1.9.5-2.9.5c1.2.4 2.4.4 3.5.2v.2l.6.8l.6-.8v-.2c1.1.2 2.3.2 3.5-.2c-1 0-2-.1-2.9-.5c1.5-.8 2.7-2.2 3.3-3.9"> </path> <path d="M38.7 29.1c-.8-1.3-2.1-2.3-3.6-2.7c3.3 2.5 4 7.2 1.5 10.5c2.8-1.5 3.7-5 2.1-7.8"> </path> <path d="M28.8 26.4c-1.5.4-2.8 1.3-3.6 2.7c-1.6 2.7-.6 6.2 2.1 7.8c-2.4-3.3-1.8-8 1.5-10.5"> </path> <path d="M30.5 26c.5.3 1.2.2 1.5-.3c.3.5 1 .6 1.5.3c.4-.3.6-.8.4-1.3c-.1.6-.6.9-1.2.8c-.3-.1-.5-.2-.7-.5c-.1.3-.4.4-.7.5c-.6.1-1.1-.3-1.2-.8c-.2.5 0 1 .4 1.3"> </path> </g> </g>
                  </svg>
                  تومان
                </button>
                <div class="relative w-full">
                  <input type="number" 
                    id="currency-input" 
                    class="block p-2.5 w-full z-20 text-sm text-gray-900 bg-gray-50 rounded-e-lg border-s-gray-50 border-s-2 border border-gray-300 focus:border-green-500 focus:outline-none" 
                    placeholder="مبلغ وارد نماید" 
                    :value="formData.amount"
                    @input="handleInputAmount"
                    @paste="handlePasteAmount"
                    @blur="checkAmount"
                    :step="inputStep"
                    :class="{ alert: hasErrorCurrency}"
                    required />
                </div>
              </div>
              <div class="relative mb-10">
                <input id="price-range-input" 
                    type="range" 
                    :value="formData.amount" 
                    :min="minValue" 
                    :max="maxValue" 
                    :step="inputStep * 100"
                    class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
                    @input="handleRangeChange"
                    />
                <span class="text-xs text-gray-500 absolute start-0 -bottom-6">حداقل {{ minValue }}</span>
                <span class="text-xs text-gray-500 absolute end-0 -bottom-6">حداکثر {{ maxValue }}</span>
              </div>
            </div>

            <!-- Phone number input area -->
            <div class="relative pt-5 mb-4">
              <label for="card-number-input" class="mb-4 block text-sm font-medium text-gray-900"> لطفا شماره موبایل تان وارد نمایд</label>
              <div class="flex items-center">
                <button id="dropdown-phone-button" data-dropdown-toggle="dropdown-phone" class="flex-shrink-0 z-10 inline-flex items-center py-2.5 px-4 text-sm font-medium text-center text-gray-900 bg-gray-100 border border-gray-300 rounded-s-lg hover:bg-gray-200 focus:ring-4 focus:outline-none focus:ring-gray-100" type="button">
                  98+
                </button>
                <div class="relative w-full">
                  <input type="text" 
                    id="phone-input" 
                    aria-describedby="helper-text-explanation" 
                    class="block p-2.5 w-full z-20 text-sm text-gray-900 bg-gray-50 rounded-e-lg border-s-0 border border-gray-300 focus:border-green-500 focus:outline-none" 
                    pattern="[0-9]{3}-[0-9]{3}-[0-9]{2}-[0-9]{2}" 
                    placeholder="123-456-78-90"
                    @input="formatPhoneNumber"
                    @blur="checkPhoneNumber"
                    :class="{ alert: hasErrorPhone}"
                    required />
                </div>
            </div>
              <!-- Helper text for phone input -->
              <p id="helper-text-explanation" class="mt-4 mb-2 text-sm text-gray-500">کد اس ام اس به شماره موبایل تان ارسال شود</p>
              <!-- Button to submit phone number -->
              <button type="submit" class="text-white w-full bg-gray-700 hover:bg-gray-800 focus:ring-4 focus:ring-blue-300 font-bold rounded-lg text-sm px-5 py-2.5 me-1 focus:outline-none">ارسال کد</button>
            </div>

            <!-- Code input area -->
            <div class="relative">
              <div>
                <label for="default-input" class="block mb-2 text-sm text-center font-medium text-gray-900">کد ارسال شده وارد نماید</label>
                <input type="text" 
                    id="default-input" 
                    class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:outline-none focus:border-green-500 block w-44 mx-auto p-2.5 text-center mb-10"
                    @input="formatCodeNumber"
                    maxlength="4"
                    />
              </div>
            </div>
            <!-- Submit button for the form -->
            <button @click="goToNextStep" type="submit" class="block mx-auto rounded-lg bg-green-600 px-10 py-2.5 font-bold text-white hover:bg-primary-800 focus:outline-none focus:ring-4 focus:ring-primary-300">ادامه</button>
          </div>
        </form>
</template>

<style scoped>
.alert {
    border: 2px solid red;
}
</style>
