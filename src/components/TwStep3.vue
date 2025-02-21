<script setup>
import { ref } from 'vue';
import TwStep3 from './TwStep3.vue';
import { banks } from '../other/banks.js'

const props = defineProps(['data']);
const emit = defineEmits(['next']);

const formData = ref({});    

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






const goToNextStep = () => {
    if (formData.value.accountNumber.length === 16 && formData.value.phoneNumber.length === 12 && formData.value.amount ) {
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
                    <span class="size-6 rounded-full bg-green-600 text-white text-center text-[12px]/6 font-bold">1</span>
                    <span class="hidden sm:block">کارت بانکی</span>
                  </li>
                  <li class="flex items-center gap-2 bg-white p-2">
                    <span class="size-6 rounded-full bg-green-600 text-white text-center text-[12px]/6 font-bold">2</span>
                    <span class="hidden sm:block">شماره حساب</span>
                  </li>
                  <li class="flex items-center gap-2 bg-white p-2">
                    <span class="size-6 rounded-full bg-green-600 text-white text-center text-[12px]/6 font-bold">3</span>
                    <span class="hidden sm:block">واریز</span>
                  </li>
                </ol>
              </div>
            </div>
            <!-- Card number input area -->
            <div class="flex flex-col items-center mt-5">
              <label for="card-number-input" class="block text-center text-lg font-medium text-gray-900">
              واریز حساب به حساب از طریق بانک {{ formData.bank_info.bank_title}}
              </label>
              <img class="h-10 my-2" :src="getBankLogo(formData.bank_info.bank_logo)" alt="Bank Logo" />
            </div>
            <!-- payment details -->
            <div class="relative">
              <div class="space-y-4 rounded-lg border border-gray-100 bg-amber-100 p-6">
            <div class="space-y-2">
              <!-- Repeated structure for displaying information labels and values -->
              <dl class="flex items-center justify-between gap-4">
                <dt class="text-base font-normal text-gray-500">لطفا مبلغ </dt>
                <dd class="text-base font-medium text-green-500">{{ formData.amount }} تومان</dd>
              </dl>
              <dl class="flex items-center justify-between gap-4">
                <dt class="text-base font-normal text-gray-500">از حساب مبدا بانک {{ formData.bank_info.bank_title}}</dt>
                <dd class="text-base font-medium text-gray-900">123456789</dd>
              </dl>
              <dl class="flex items-center justify-between gap-4">
                <dt class="text-base font-normal text-gray-500">به حساب مقصد بانک {{ formData.bank_info.bank_title}} انتقال انجام بدید</dt>
                <dd class="text-base font-bold text-orange-600">123456789</dd>
              </dl>
            </div>
            <!-- Additional transaction state information with bold styling -->
            <dl class="pt-5 flex items-center justify-between gap-4 border-t border-gray-200 pt-2">
              <dt class="text-sm font-bold text-gray-900">طفاً پس از انتقال به مشخصات حسابی که ذکر شده است، از انتقال خود یک اسکرین‌شات یا رسید بگیرید و آن را در فرم زیر بارگذاری کنید (در صورتی که پرداخت به صورت خودکار یافت نشود، مورد نیاز خواهد بود) و دکمه پرداخت انجام شده را فشار دهید.

              <p class="pt-3 text-yellow-800">توجه داشته باشید! پرداخت طی 15 دقیقه واریز می‌شود. مراجعه به خدمات پشتیبانی قبل از این زمان ممکن نیست. در مورد پرداخت‌های واریز نشده، دقیقاً پس از گذشت 15 دقیقه به پشتیبانی پیام دهید</p>  
              </dt>
              
              
            </dl>
          </div>

            

            </div>
            <!-- Dropzone  -->
            <div class="flex items-center justify-center w-full">
                <label for="dropzone-file" class="flex flex-col items-center justify-center w-full h-44 border-2 border-gray-300 border-dashed rounded-lg cursor-pointer bg-gray-50 hover:bg-gray-100">
                <div class="flex flex-col items-center justify-center pt-5 pb-6">
                   <svg class="w-8 h-8 mb-4 text-gray-500" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 16">
                   <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 13h3a3 3 0 0 0 0-6h-.025A5.56 5.56 0 0 0 16 6.5 5.5 5.5 0 0 0 5.207 5.021C5.137 5.017 5.071 5 5 5a4 4 0 0 0 0 8h2.167M10 15V6m0 0L8 8m2-2 2 2"/>
                   </svg>
                  <p class="mb-2 text-sm text-gray-500"><span class="font-semibold">Click to upload</span> or drag and drop</p>
                  <p class="text-xs text-gray-500">SVG, PNG, JPG or GIF (MAX. 800x400px)</p>
               </div>
              <input id="dropzone-file" type="file" class="hidden" />
              </label>
            </div> 

          </div>
          <!-- Submit button for the form -->
          <button type="submit" class="block mx-auto rounded-lg bg-green-600 px-10 py-2.5 font-bold text-white hover:bg-primary-800 focus:outline-none focus:ring-4 focus:ring-primary-300">انتقال را انجام دادم</button>
          <!-- progress -->
          <div class="mt-14 mb-5 w-full bg-gray-200 rounded-full">
           <div class="bg-green-600 text-xs font-medium text-blue-100 text-center p-0.5 leading-none rounded-full" style="width: 45%"> 45%</div>
          </div>
        </form>
</template>

<style scoped>
.alert {
    border: 2px solid red;
}
</style>
