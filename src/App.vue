<template>
  <div>Выбрать режим</div>
  <select v-model="selectedMode">
    <option>default</option>
    <option>closable</option>
    <option>opened</option>
  </select>
  <div v-if="accordionData">
    <accordion v-if="accordionData.length" :data="accordionData" :mode="selectedMode" />
    <div v-else>Данные для вывода отсутствуют</div>
  </div>
  <div v-else> {{ errorText }}</div>
  <VLoader v-if="isLoading" />
</template>

<script>
import Accordion from './components/Accordion/Accordion.vue'
import VLoader from './components/Loader/VLoader.vue'

import { onMounted, ref } from 'vue'
import { getPosts } from './api/postsApi'
export default {
  name: 'App',
  components: {
    Accordion,
    VLoader
  },

  setup () {
    const accordionData = ref(null)

    const errorText = ''
    const isLoading = ref(false)
    const selectedMode = ref('default')

    onMounted(async () => {
      isLoading.value = true
      const response = await getPosts()
      if (response) {
        accordionData.value = response
      } else {
        errorText.value = 'Ошибка загрузки данных'
      }

      isLoading.value = false
    })

    return { accordionData, errorText, selectedMode }
  }
}
</script>
