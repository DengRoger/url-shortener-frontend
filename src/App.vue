
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
    import axios from 'axios';
    export default {
    data() {
      return {
        inputUrl: '',
        originalUrl: '',
        shortenedUrl: '',
        isUrlValid: true
      };
    },
    created() {
      const currentUrl = window.location.href;
      const parts = currentUrl.split("/");
      const lastPart = parts[parts.length - 1];
      console.log(lastPart);
      if(lastPart != ""){
        axios.get("https://api.rogerdeng.net/api/query?id="+lastPart)
        .then(response=>{
          console.log(response);
          if(response.data.successful){
            window.location.href = response.data.original_url;
            //this.shortenedUrl = "https://url.rogerdeng.net/"+response.data.short_url;
          }
          else{
            alert(404);
          }
        })
        .catch(error=>{
          console.log(error.response);
        });
      }
    },
    methods: {
      shortenUrl() {
        if (!this.isValidUrl(this.inputUrl)) {
          this.isUrlValid = false;
          return;
        }
        this.isUrlValid = true;

        this.originalUrl = this.inputUrl;
        axios.post("https://api.rogerdeng.net/api/insert",{
          original_url: this.originalUrl
        })
        .then(response=>{
          if(response.data.successful){
            this.shortenedUrl = "https://url.rogerdeng.net/"+response.data.short_url;
          }
        })
        .catch(error=>{
          console.log(error.response);
        })
      },
      isValidUrl(url) {
        //eslint-disable-next-line
        const urlPattern = /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})(:[1-9]\d{0,4})?([\/\w \.-]*)*\/?$/
        return urlPattern.test(url);
      }
      
    }
    
  };
</script>
