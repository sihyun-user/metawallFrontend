<template>
  <section class="upload-userPost">
    <base-caption>張貼動態</base-caption>
    <base-card class="post">
      <div class="post__text">
        <h1>請填寫用戶ID(測試ID:627dc7f2244ef76fadcf7207)</h1>
        <input type="text" class="inputMode" placeholder="輸入您的用戶ID(測試API用，可使用api-doc文件生成)" v-model="userID">
      </div>
      <div class="post__text">
        <h1>貼文內容</h1>
        <textarea placeholder="輸入您的貼文內容" v-model="content"></textarea>
      </div>
      <div class="post__photo">
        <button class="post__photo--btn">
          <label for="upload">
            上傳圖片
            <input id="upload" type="file" accept="image/*" @input="handlePreviewImage">
          </label>
        </button>
        <div class="post__photo--img" v-if="previewImage">
          <img :src="previewImage">
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
import axios from 'axios'
import BaseCaption from '../components/ui/BaseCaption.vue'
import BaseCard from '../components/ui/BaseCard.vue'
export default {
  components: {
    BaseCaption, BaseCard
  },
  setup() {
    const content = ref('')
    const store = useStore()
    const previewImage = ref(null)
    const userID = ref('') // API測試用

    const errorMag = computed(() => store.getters.errorMag)

    // 預覽圖檔
    function handlePreviewImage (e) {
      const file = e.target.files[0]

      const reader = new FileReader()
      reader.readAsDataURL(file)
      reader.onload =(e => {
        previewImage.value = e.target.result
      })
    }

    // 建立貼文
    async function submitPost () {
      try {
        if(content.value==='' || userID.value==='') {
          const errorTxt = '貼文內容或用戶ID不得為空'
          return store.commit('setErrorMag', errorTxt)
        }

        const paramData = {
          content: content.value,
          image: previewImage.value,
          user: userID.value
        }

        const api = `${process.env.VUE_APP_API}/posts`
        const response = await axios.post(api, paramData)

        console.log(response)
      } catch (error) {
        alert('系統忙碌中，請稍後再試')
      }
    }


    return {
      content,
      errorMag,
      previewImage,
      userID,
      handlePreviewImage,
      submitPost
    }
  }
}
</script>
