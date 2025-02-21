<script setup>
import { ref } from 'vue';
import TwStep2 from './TwStep2.vue';
import { banks } from '../other/banks.js'

const props = defineProps(['data']);
const emit = defineEmits(['next']); 

const cardNumber = ref('');
const bankLogo = ref('shetab.svg');
const hasError = ref(false);

const goToNextStep = () => {

    if (cardNumber.value.length === 16) {
        const selectBank = detectCard(cardNumber.value);
        if(selectBank.hasOwnProperty('card_no')){
            hasError.value = false;
            console.log({
                cardnumber: cardNumber.value,
                bank_info: selectBank,
            });
            emit('next', TwStep2, {
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

// 

function formatCardNumber(event) {
    let value = event.target.value.replace(/\D/g, '');
    value = value.match(/.{1,4}/g)?.join(' ') || '';
    event.target.value = value;
    cardNumber.value = value.replace(/\s/g, '');
    handleBankLogo();
    if(cardNumber.value.length < 16) {
        hasError.value = false;
    }
}

function handleBankLogo() {
    if (cardNumber.value.length >= 6) {
        const selectBank = detectCard(cardNumber.value);
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
    if(cardNumber.value.length < 16) {
        hasError.value = false;
    }
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
function detectCard(cardNumber) {
    const firstCardNumber = cardNumber.slice(0, 6);
    const selectBank = banks.filter((item) => item.card_no == firstCardNumber);
    if(selectBank.length) {
        return selectBank[0];
    } else {
        return {};
    }
}
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
                    <span class="size-6 rounded-full bg-green-600 text-white text-center text-[12px]/6 font-bold">1</span>
                    <span class="hidden sm:block">کارت بانکی</span>
                  </li>
                  <li class="flex items-center gap-2 bg-white p-2">
                    <span class="size-6 rounded-full bg-gray-100 text-center text-[10px]/6 font-bold">2</span>
                    <span class="hidden sm:block">شماره حساب</span>
                  </li>
                  <li class="flex items-center gap-2 bg-white p-2">
                    <span class="size-6 rounded-full bg-gray-100 text-center text-[10px]/6 font-bold">3</span>
                    <span class="hidden sm:block">واریز</span>
                  </li>
                </ol>
              </div>
            </div>
            <!-- Card number input area -->
            <label for="card-number-input" class="block text-sm font-medium text-gray-900">لطفا شماره کارت تان وارد نماید</label>
            <div class="relative">
              <input dir="ltr" type="text" id="card-number-input" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:outline-none focus:border-green-500 block w-full pe-10 p-2.5 text-right sm:text-center" 
                placeholder="4242 4242 4242 4242" 
                @input="formatCardNumber"
                @paste="handlePaste"
                maxlength="19"
                :class="{ alert: hasError }"
                required>
              <!-- design details - icon all irr banks -->
              <div class="absolute inset-y-0 end-0 top-0 flex items-center pe-3.5 pointer-events-none">
                <img v-if="bankLogo" class="h-8 w-auto" :src="getBankLogo(bankLogo)" alt="" />
              </div>
            </div>
          </div>
          <!-- Submit button for the form -->
          <button @click="goToNextStep" type="submit" class="block mx-auto rounded-lg bg-green-600 px-10 py-2.5 font-bold text-white hover:bg-primary-800 focus:outline-none focus:ring-4 focus:ring-primary-300">ادامه</button>
        </form>
</template>

<style scoped>
.alert {
    border: 2px solid red;
}
</style>
