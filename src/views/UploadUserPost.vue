<template>
  <section class="upload-userPost">
    <base-caption>張貼動態</base-caption>
    <base-card class="post">
      <div class="post__text">
        <h1>貼文內容</h1>
        <textarea placeholder="輸入您的貼文內容" v-model="content"></textarea>
      </div>
      <div class="post__photo">
        <button class="post__photo--btn">上傳圖片</button>
        <div class="post__photo--img">
          <img src="">
        </div>
      </div>
      <p class="error" v-if="errorMag">
        <!-- 圖片檔案過大，僅限 1mb 以下檔案<br>
        圖片格式錯誤，僅限 JPG、PNG 圖片 -->
        {{ errorMag }}
      </p>
      <div class="post__sunbmitBtn">
        <button class="baseGrayBtn" @click="submitPost">送出貼文</button>
      </div>
    </base-card>
  </section>
</template>
<script>
import { ref, computed } from 'vue'
import { useStore } from 'vuex'
import BaseCaption from '../components/ui/BaseCaption.vue'
import BaseCard from '../components/ui/BaseCard.vue'
export default {
  components: {
    BaseCaption, BaseCard
  },
  setup() {
    const content = ref('')
    const store = useStore()

    const errorMag = computed(() => store.getters.errorMag) 

    async function submitPost () {
      try {
        if(content.value==='') {
          const errorTxt = '貼文內容不得為空'
          return store.commit('setErrorMag', errorTxt)
        }
      } catch (error) {
        alert('系統忙碌中，請稍後再試')
      }
    }

    return {
      content,
      errorMag,
      submitPost
    }
  }
}
</script>
