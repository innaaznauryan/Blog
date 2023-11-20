<template>
  <div class="pt-20 text-teal-800 font-sans text-center">
    <div v-if="postError" class="text-red-500">{{ postError }}</div>
    <div
        v-else-if="posts"
        class="flex flex-wrap justify-center">
      <router-link
          v-for="post in posts"
          :to="{name: 'single-post', params: {id: post.id}}"
          class="w-full sm:w-1/2 lg:w-1/4 xl:w-1/5 p-2">
        <div class="h-full bg-white shadow-lg">
          <h2 class="p-2 h-20 font-medium text-lg bg-orange-400 shadow-md grid place-content-center">{{ post.title }}</h2>
          <div class="p-4 h-64 duration-500 hover:text-orange-400">
            <h3 class="font-medium text-lg">{{ post.user }}</h3>
            <p class="pt-2 italic">{{ post.date }}</p>
            <p>{{ post.summary }}</p>
          </div>
        </div>
      </router-link>
    </div>
    <BaseButton
        v-if="loggedIn"
        @click="handleClick"
        class="fixed border-2 border-teal-800 hover:border-orange-400"
        :customClass="{'bg-teal-500': true}"
        :style="{ right: '20px', bottom: '20px' }">
      Create Post
    </BaseButton>
    <teleport to="#modal" :disabled="!showModal" v-if="showModal">
      <CreatePost v-if="loggedIn" :scrollTop="scrollTop"/>
    </teleport>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue"
import { posts, getPosts, showModal, postError } from "@/composable/usePosts"
import { getLoggedIn, loggedIn } from "@/composable/useUsers"
import CreatePost from "@/components/CreatePost.vue"
import BaseButton from "@/components/BaseButton.vue"

const scrollTop = ref(null)
const handleClick = () => {
  showModal.value = true
  scrollTop.value = window.scrollY
}
onMounted(async() => {
  showModal.value = false
  await getPosts()
  await getLoggedIn()
})
</script>

<style scopped>

</style>