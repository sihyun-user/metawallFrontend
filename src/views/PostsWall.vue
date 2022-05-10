<template>
  <section class="posts-wall">
    <post-filter class="filter" @filter-posts='searchPosts'></post-filter>

    <section v-if="!isLoading">
      <div class="no-post" v-if="posts.length==0">目前尚無動態，新增一則貼文吧！</div>
      <post-card v-else v-for="post in posts"
      :key="post._id"
      :user="post.user"
      :content="post.content"
      :postImage="post.image"
      >
      </post-card>
    </section>

    <base-spinner v-else></base-spinner>
  </section>
</template>

<script>
import { computed, ref } from 'vue'
import { useStore } from 'vuex'
// import axios from 'axios'
// import handleService from '../service/helpers'
import PostCard from '../components/PostCard.vue'
import PostFilter from '../components/PostFilter.vue'
export default {
  components: {
    PostCard, PostFilter
  },
  setup () {
    const store = useStore()
    const posts = ref([])
    let sortData = {
      sortTime: 'desc',
      content: ''
    }

    const isLoading = computed(() => store.getters.isLoading)

    // 搜尋貼文
    function searchPosts (val) {
      sortData = {...val}
      getPosts()
    }

    // 撈取貼文
    async function getPosts () {
      try {
        const paramData = {
          sortTime: sortData.sortTime,
          content: sortData.content
        }
        
        console.log(paramData)

        // store.commit('setIsLoading', true)
        // const api = 'https://aqueous-lake-66228.herokuapp.com/posts'
        // const res = await axios.get(api, paramData)
        // store.commit('setIsLoading', false)
        // if (res.data.status) {
        //   handleService.checkConsole('獲取貼文成功', res.data)
        //   posts.value = res.data.data
        // }
      } catch (error) {
        alert('系統忙碌中，請稍後再試')
      }
    }

    return {
      posts,
      isLoading,
      getPosts,
      searchPosts
    }
  }
}
</script>
