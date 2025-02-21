<script setup>
import TwStep1 from './TwStep1.vue'
let currentComponent = ref(TwStep1);
const currentData = ref({});

const handleNext = (nextComponent, data = {}) => {
  currentComponent.value = nextComponent;
  currentData.value = data;
};



import { ref } from 'vue'
import { banks } from '../other/banks.js'


const cardNumber = ref('');
const bankLogo = ref('shetab.svg');

function formatCardNumber(event) {
    let value = event.target.value.replace(/\D/g, '');
    value = value.match(/.{1,4}/g)?.join(' ') || '';
    event.target.value = value;
    cardNumber.value = value.replace(/\s/g, '');
    handleBankLogo();
}

function handleBankLogo() {
    if (cardNumber.value.length >= 6) {
        const selectBank = detectCard(cardNumber.value);
        console.log(selectBank);
        if (selectBank.hasOwnProperty('bank_logo')) {
            bankLogo.value = selectBank.bank_logo;
        } else {
            bankLogo.value = 'shetab.svg';
        }
    } else {
        bankLogo.value = 'shetab.svg';
    }
}


function handlePaste(event) {
    event.preventDefault();
    const pastedData = event.clipboardData.getData('text').replace(/\D/g, '').slice(0, 16);
    const formattedData = pastedData.match(/.{1,4}/g)?.join(' ') || '';
    event.target.value = formattedData;
    cardNumber.value = formattedData.replace(/\s/g, '');
    handleBankLogo();
}
const getBankLogo = (logo) => {
    try {
      if(logo.length) {
        return new URL(`../assets/images/banks/${logo}`, import.meta.url).href;
      }
      return '';
    } catch (error) {
        console.error('Error loading logo:', error);
        return '';
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

</script>

<template>
<section dir="rtl" class="bg-slate-400 py-8 antialiased md:py-16">
  <!-- Main container with maximum width and padding -->
  <div class="mx-auto max-w-screen-xl px-4 2xl:px-0">
    <!-- Inner container with custom styling and max width -->
    <div class="bg-slate-100 rounded-lg p-5 mx-auto max-w-5xl">
      <!-- Header section with custom background and rounded corners -->
      <header dir="rtl" class="bg-sky-950 rounded-md">
        <!-- Header content area with horizontal padding and flexbox layout -->
        <div class="mx-auto flex h-16 max-w-screen-xl items-center gap-8 px-4 sm:px-6 lg:px-8">
          <!-- Logo link with hidden text for accessibility and SVG logo -->
          <a class="block text-green-600" href="#">
            <span class="sr-only">Home</span>
            <!-- SVG Placeholder for logo -->
            <svg width="52px" height="52px" viewBox="-10.24 -10.24 84.48 84.48" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" aria-hidden="true" role="img" class="iconify iconify--emojione" preserveAspectRatio="xMidYMid meet" fill="#ffffff">
              <!-- SVG paths omitted for brevity -->
              <path transform="translate(-10.24, -10.24), scale(5.28)" fill="#0d910f" d="M9.166.33a2.25 2.25 0 00-2.332 0l-5.25 3.182A2.25 2.25 0 00.5 5.436v5.128a2.25 2.25 0 001.084 1.924l5.25 3.182a2.25 2.25 0 002.332 0l5.25-3.182a2.25 2.25 0 001.084-1.924V5.436a2.25 2.25 0 00-1.084-1.924L9.166.33z" strokewidth="0"></path><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round" stroke="#CCCCCC" stroke-width="0.128"></g><g id="SVGRepo_iconCarrier"> <path d="M32.1 8.9s-.1 0 0 0h-.2C6.9 8.9 2 17.7 2 31.5l.9 9.4h58.3l.9-9.4c-.1-13.8-5-22.6-30-22.6" fill="#404040"> </path> <path d="M58.5 36.8c-9.1-3.6-2-13.5-1.7-15.5C56.8 21.3 54.7 2 32 2S7.2 21.3 7.2 21.3c.3 2 7.5 11.9-1.7 15.5c0 0-3.4-.4-3.4 3.7c0 4.9 3.5 4.1 3.5 4.1C5.6 52.1 13 62 32 62s26.4-9.9 26.4-17.4c0 0 3.5.8 3.5-4.1c0-4.1-3.4-3.7-3.4-3.7" fill="#fff5db"> </path> <g fill="#404040"> <path d="M40.7 52.9c.2 0 .3-.9.3-1.3c0-2.6-4-4.8-9-4.8s-9 2.3-9 4.9c0 .3.1 1.2.2 1.2h17.5"> </path> <path d="M17.6 9.8c2.3-.7 4.7-1.5 7.1-2c2.4-.5 4.9-.9 7.3-1c2.5-.1 5 .1 7.4.6c2.4.5 4.8 1.3 7 2.4c-2.4-.4-4.8-.8-7.2-.9c-2.4-.2-4.8-.2-7.1-.1c-2.4.1-4.8.3-7.2.5c-2.4.2-4.8.5-7.3.5"> </path> <path d="M14.5 14c2.9-.7 5.7-1.5 8.6-2c2.9-.5 5.9-.9 8.9-1c3-.1 6 .1 8.9.6c3 .5 5.9 1.2 8.6 2.4c-2.9-.4-5.8-.7-8.8-.9c-2.9-.2-5.8-.2-8.7-.1c-2.9.1-5.8.3-8.7.5c-2.9.2-5.8.6-8.8.5"> </path> </g> <g fill="#3da90f"> <path d="M17.8 39.3l-.3-3c.8-.1 1.9-.3 3.1-.5c5.9-1.1 15.7-3 26.4.6l-1 2.8c-9.9-3.4-19.2-1.6-24.8-.5c-1.4.3-2.6.5-3.4.6"> </path><circle cx="19.7" cy="37.9" r="9"> </circle> </g> <circle cx="19.7" cy="37.9" fill="#fff5db" r="6"> </circle> <circle cx="44.3" cy="37.9" fill="#3da90f" r="9"> </circle> <circle cx="44.3" cy="37.9" fill="#fff5db" r="6"> </circle> </g>
            </svg>
          </a>
          <!-- Flex container for navigation and other elements -->
          <div class="flex flex-1 items-center justify-end md:justify-between">
            <!-- Placeholder for navigation (hidden on mobile) -->
            <nav aria-label="Global" class="hidden md:block"></nav>
            <!-- Flex container for additional interactive elements -->
            <div class="flex items-center gap-4">
              <!-- Button with text, icon, and styling -->
              <button type="button" class="text-white bg-green-600 hover:bg-green-700 font-bold rounded-md text-sm px-5 py-2.5 text-center inline-flex items-center">
                <!-- SVG icon inside button -->
                <svg class="me-1 w-6 h-6" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
                   <path fill-rule="evenodd" d="M12 2a7 7 0 0 0-7 7 3 3 0 0 0-3 3v2a3 3 0 0 0 3 3h1a1 1 0 0 0 1-1V9a5 5 0 1 1 10 0v7.083A2.919 2.919 0 0 1 14.083 19H14a2 2 0 0 0-2-2h-1a2 2 0 0 0-2 2v1a2 2 0 0 0 2 2h1a2 2 0 0 0 1.732-1h.351a4.917 4.917 0 0 0 4.83-4H19a3 3 0 0 0 3-3v-2a3 3 0 0 0-3-3 7 7 0 0 0-7-7Zm1.45 3.275a4 4 0 0 0-4.352.976 1 1 0 0 0 1.452 1.376 2.001 2.001 0 0 1 2.836-.067 1 1 0 1 0 1.386-1.442 4 4 0 0 0-1.321-.843Z" clip-rule="evenodd"/>
                </svg>
                راهنمای
              </button>
            </div>
          </div>
        </div>
      </header>

      <!-- Title for the section (hidden on mobile) -->
      <h2 class="text-xl text-center font-semibold text-gray-900 sm:text-2xl m-8 hidden sm:block">واریز ریالی</h2>

      <!-- Main content area with form and information -->
      <div class="mt-6 sm:mt-8 lg:flex lg:items-start lg:gap-12">
        <!-- Form for financial transaction inputs - card number-->
        <component :is="currentComponent" :data="currentData" @next="handleNext" />
        <!-- Information display area with transaction details -->
        <div class="mt-6 grow sm:mt-8 lg:mt-0">
          <div class="space-y-4 rounded-lg border border-gray-100 bg-gray-50 p-6">
            <div class="space-y-2">
              <!-- Repeated structure for displaying information labels and values -->
              <dl class="flex items-center justify-between gap-4">
                <dt class="text-base font-normal text-gray-500">شماره درخواست</dt>
                <dd class="text-base font-medium text-gray-900">ساخته نشد</dd>
              </dl>
              <dl class="flex items-center justify-between gap-4">
                <dt class="text-base font-normal text-gray-500">مبلغ</dt>
                <dd class="text-base font-medium text-green-500">****** تومان</dd>
              </dl>
           </div>

            <!-- Additional transaction state information with bold styling -->
            <dl class="flex items-center justify-between gap-4 border-t border-gray-200 pt-2">
              <dt class="text-base font-bold text-gray-900">حالت</dt>
              <dd class="text-base font-bold text-gray-900">در حال اجرا</dd>
            </dl>
           </div>
          <!-- Display area for bank logos  -->
          <div class="mt-5 flex items-center justify-center gap-8">
            <!-- Repeated elements for displaying bank logos -->
            <img class="h-8 w-auto" src="../assets/images/banks/melli-22.svg" alt="" />
            
            <img class="h-8 w-auto" src="../assets/images/banks/mellat-02.svg" alt="" />
            
            <img class="h-8 w-auto" src="../assets/images/banks/sepah-24.svg" alt="" />
           
           <img class="h-8 w-auto" src="../assets/images/banks/tejarat-13.svg" alt="" />
           <img class="h-8 w-auto" src="../assets/images/banks/keshavarzi-07.svg" alt="" />
          </div>
        </div>
      </div>
      <!-- Footer area with copyright text -->
      <p class="mt-6 text-center text-gray-500 sm:mt-8 lg:text-left">
        AmuPay ©2024
      </p>
    </div>
  </div>
</section>
</template>

<style scoped>
</style>
