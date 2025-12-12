<script setup>
import { ref } from "vue";
import axios from "axios";
import TrackingTimeline from "./components/TrackingTimeline.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

const trackingType = ref("");
const trackingNumber = ref("");
const token_tbox =
  "eyJhbGciOiJBMTI4S1ciLCJlbmMiOiJBMTI4Q0JDLUhTMjU2IiwidHlwIjoiSldUIiwiY3R5IjoiSldUIn0.irwDw03J7vIUl0yPXiVLHzfIUirp-bWyIf_W-gRWb3zm6CI4-P8u4w.30rPiEuBZ8xAapjuWrpGvQ.6RtRZGPT7Wanjt9idg-cLQWCm8Vday9rmckscNXg1Xv978LSPU4Olt7Mm3eSmszUM3EL-Jr1jRe7TAAXUpV-ULsjdF9uZTnDFngNbFyIJC7MMxZNDdzIGP6KcNgTrQlMclktKkNNUc7IPJiyZKaakY5J0Wwpn4R1ia8JAStmtJc-cbwnIXYK3Sw_rkjfmPyVq-C7fgq8QmdyAzqj6sjOBYPcZbU56SWtHMF8jnTdJfBURgiDDayRaftQAAWQ7Y5yVkSFh97YbSJ5aTMTdRSSrVBmVImsxFBdGaz_d3Dj4obOVN3giGM1VKopg3X_bmNgMryjegi9EaBJUnIYWIfNblG18IxHOcB25722rD4gEd7TB-h_VjlL9N6H6q_zUJ2tIToEq-btq9ZvX7pn6GeVphEvYhFLmPqWidjQoivr6cl4rV03xbCINcycP0_TX2IETEbg_deNuUhJWbZvHi-2JL1wfuzruTEU5sysUvjPZ0TinjYlIAGHYebl_H5tAu7D2d7mhZ3I61CWFzqixDvvocd726QGN8aYbrKXulu91wowEJKQX9HW3vzYdqJ99ggI.MD81Lj7c_SdUYFfDU373sQ"; // توکن شما

const result = ref(null);
const isLoading = ref(false);
const error = ref(null);

const showWave = ref(false);
const animateWave = () => {
  showWave.value = true;
  setTimeout(() => {
    showWave.value = false;
  }, 1000);
};

const setTrackingType = (type) => {
  trackingType.value = type;
};

const trackPost = async () => {
  try {
    isLoading.value = true;
    const { data } = await axios.get(
      `https://api.postex.ir/api/app/v1/tracking/public/${trackingNumber.value}`
    );
    result.value = data;
  } catch (error) {
    console.error("Error:", error.response?.data || error.message);
    alert("خطا در دریافت اطلاعات");
  } finally {
    isLoading.value = false;
  }
};

const trackTipax = async () => {
  try {
    isLoading.value = true;
    const { data } = await axios.get(
      `https://omtestapi.tipax.ir/api/OM/v3/Tracking/BriefTracking/${trackingNumber.value}}`,
      {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      }
    );
    result.value = data;
  } catch (error) {
    console.error("Error:", error.response?.data || error.message);
    alert("خطا در دریافت اطلاعات");
  } finally {
    isLoading.value = false;
  }
};

const trackMahex = async () => {
  try {
    isLoading.value = true;
    const { data } = await axios.get(
      `https://mahex.com/website/api/web/v1/tr/${trackingNumber.value}`
    );
    result.value = data;
  } catch (error) {
    console.error("Error:", error.response?.data || error.message);
    alert("خطا در دریافت اطلاعات");
  } finally {
    isLoading.value = false;
  }
};

const getData = () => {
  if (!trackingNumber.value) {
    error.value = "شماره رهگیری را وارد کنید";
    return;
  }

  result.value = null; // Reset previous result
  error.value = null;

  switch (trackingType.value) {
    case "iran_post":
      trackPost();
      break;
    case "tipax":
      trackTipax();
      break;
    case "mahex":
      trackMahex();
    default:
      error.value = "شرکت پستی انتخاب نشده";
  }
};
</script>

<template class="min-h-screen bg-gray-800">
  <!-- هدر با انیمیشن -->
  <Header />

  <!-- بخش اصلی -->
  <main class="container px-4 py-10 mt-14" dir="rtl">
    <div class="max-w-3xl mx-auto bg-gray-900 rounded-xl shadow-lg overflow-hidden">
      <!-- کارت رهگیری -->
      <div class="p-6 sm:p-8">
        <div class="flex items-center justify-between mb-6">
          <h2 class="text-2xl font-bold text-gray-100">رهگیری مرسوله</h2>
        </div>

        <!-- ورودی کد رهگیری -->
        <div class="relative w-full max-w-md mx-auto font-sans">
          <div class="relative">
            <input
              type="text"
              v-model="trackingNumber"
              @focus="animateWave"
              @input="validateTracking"
              placeholder=" "
              class="w-full px-4 py-4 pr-12 bg-white/5 backdrop-blur-sm border border-gray-700/50 rounded-xl text-gray-100 text-base text-right font-mono tracking-wider focus:outline-none focus:border-primary/80 focus:ring-2 focus:ring-primary/30 transition-all duration-300 peer placeholder-shown:text-right"
              dir="ltr"
              inputmode="numeric"
              pattern="[0-9]*"
            />

            <!-- Label متحرک - بالاتر رفته -->
            <label
              class="absolute right-4 top-1.5 text-gray-500 text-xs font-medium pointer-events-none transition-all duration-300 peer-placeholder-shown:text-base peer-placeholder-shown:top-1/2 peer-placeholder-shown:-translate-y-1/2 peer-placeholder-shown:text-gray-400 peer-focus:top-1.5 peer-focus:text-xs peer-focus:text-primary peer-focus:translate-y-0 peer-not-placeholder-shown:top-1.5 peer-not-placeholder-shown:text-xs peer-not-placeholder-shown:translate-y-0"
            >
              کد رهگیری مرسوله
            </label>

            <!-- شمارنده کاراکتر - با فاصله بیشتر از آیکون -->
            <div
              class="absolute left-14 top-1/2 transform -translate-y-1/2 pointer-events-none"
            >
              <span
                class="text-xs text-gray-500 font-mono bg-gray-900/80 px-2 py-1 rounded-lg min-w-[50px] text-center"
              >
                {{ trackingNumber?.length || 0 }}/24
              </span>
            </div>

            <!-- آیکون -->
            <div
              class="absolute left-3 top-1/2 transform -translate-y-1/2 pointer-events-none"
            >
              <svg
                class="w-5 h-5 text-gray-500"
                fill="none"
                stroke="currentColor"
                viewBox="0 0 24 24"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="1.5"
                  d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"
                />
              </svg>
            </div>

            <!-- نوار انیمیشنی -->
            <div
              v-if="showWave"
              class="absolute -bottom-1 left-4 right-4 h-0.5 bg-gradient-to-r from-primary/0 via-primary to-primary/0 animate-pulse"
            ></div>
          </div>
        </div>

        <!-- انتخاب شرکت پستی -->
        <div class="mb-8">
          <label class="block text-gray-100 mb-2 font-medium">شرکت پستی</label>
          <div class="grid grid-cols-1 sm:grid-cols-3 gap-2">
            <button
              @click="setTrackingType('iran_post')"
              class="p-3 border rounded-lg hover:border-primary transition-all flex items-center justify-center focus:bg-green-500"
            >
              <img src="./icons/Iran-Post-Logo.svg" class="h-8" alt="پست ایران" />
            </button>
            <button
              @click="setTrackingType('tipax')"
              class="p-3 border rounded-lg hover:border-primary transition-all flex items-center justify-center focus:bg-green-500"
            >
              <img src="./icons/Tipax-Logo.svg" class="h-8" alt="تیپاکس" />
            </button>
            <button
              @click="setTrackingType('mahex')"
              class="p-3 border rounded-lg hover:border-primary transition-all flex items-center justify-center focus:bg-green-500"
            >
              <img src="./icons/Mahex-Logo.svg" class="h-8" alt="ماهکس" />
            </button>
          </div>
        </div>

        <!-- دکمه رهگیری -->
        <button
          @click="getData"
          :disabled="isLoading"
          class="w-full bg-secondary hover:bg-primary text-white py-4 px-6 rounded-lg font-bold text-lg transition-all transform hover:scale-[1.01] shadow-md hover:shadow-lg"
        >
          رهگیری مرسوله
          <i class="fas fa-arrow-left mr-2 animate-pulse-slow"></i>
        </button>
      </div>

      <!-- نمایش خطا -->
      <div v-if="error" class="mt-4 text-red-500">{{ error }}</div>

      <!-- نمایش نتیجه -->
      <TrackingTimeline :result="result" />
    </div>
  </main>

  <!-- فوتر -->
  <Footer />
</template>

<style>
@keyframes wave {
  0% {
    transform: scaleX(0);
    opacity: 1;
  }
  100% {
    transform: scaleX(1);
    opacity: 0;
  }
}
.animate-wave {
  animation: wave 0.8s cubic-bezier(0.65, 0, 0.35, 1) forwards;
}
</style>
