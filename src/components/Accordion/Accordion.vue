<template>
  <div>
    <div v-for="(item,ind) in data" :key="ind">
      <accordion-item
      :item="item"
      :mode="mode"
      :currentIndex = "currentIndex"
      :index="ind"
      @update:index="updateCurrenIndex"
      />
    </div>
  </div>
</template>

<script lang="ts">
import AccordionItem from './AccordionItem.vue'
import { accordionModeType, accordionItemType } from './accordion.type'
import { ref, defineComponent, PropType } from 'vue'

export default defineComponent({
  name: 'AccordionBody',
  props: {
    data: {
      type: Array as PropType<accordionItemType[]>
    },
    mode: {
      type: Object as PropType<accordionModeType>
    }
  },
  components: {
    AccordionItem
  },

  setup () {
    const currentIndex = ref(0)

    const updateCurrenIndex = (index: number) => {
      currentIndex.value = index
    }
    return { updateCurrenIndex, currentIndex }
  }
})
</script>
