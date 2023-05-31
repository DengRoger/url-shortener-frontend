
<template>
    <!-- set tailwind high 1/3 -->
    <div class="relative w-screen h-screen bg-zinc-900	">
        <div class="absolute inset-y-1/3 inset-x-1/4">
            <h1 class="text-4xl font-bold text-center text-gray-400	">URL Shortener</h1>
            <div class="py-2">
                <div>
                    <input class="w-full rounded-l px-4 py-4 focus:outline-none bg-zinc-800 text-white" v-model="inputUrl" :class="{ 'border-red-500': !isUrlValid }"  type="text" placeholder="輸入原始網址">
                </div>
                <div class="flex py-2">
                    <button class="bg-blue-500 hover:bg-blue-600 text-white font-bold px-4 py-2 rounded-lg w-full h-full" @click="shortenUrl">縮短</button>
                </div>
                <div v-if="shortenedUrl" class=" p-4 rounded bg-zinc-600 text-white">
                    <p class="mb-2"><strong>原始網址:</strong> {{ originalUrl }}</p>
                    <p><strong>縮短後網址:</strong> <a :href="shortenedUrl" target="_blank">{{ shortenedUrl }}</a></p>
                </div>
                <p v-if="!isUrlValid" class="text-red-500">請輸入有效的網址</p>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
    data() {
      return {
        inputUrl: '',
        originalUrl: '',
        shortenedUrl: '',
        isUrlValid: true
      };
    },
    methods: {
      shortenUrl() {
        // 檢查網址是否有效
        if (!this.isValidUrl(this.inputUrl)) {
          this.isUrlValid = false;
          return;
        }
        // 網址有效，執行其他邏輯
        this.isUrlValid = true;

        // 在此可以添加實際的縮短網址的邏輯
        // 這裡只是模擬，將原始網址設定為縮短後網址
        this.shortenedUrl = this.inputUrl;
        this.originalUrl = this.inputUrl;
      },
      isValidUrl(url) {
        // 使用正則表達式檢查網址格式
        //eslint-disable-next-line
        const urlPattern = /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})(:[1-9]\d{0,4})?([\/\w \.-]*)*\/?$/
        return urlPattern.test(url);
      }
    }
  };
</script>
