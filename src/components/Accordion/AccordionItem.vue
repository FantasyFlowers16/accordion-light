<template>
  <div class="accordion-item">
    <div @click="openItem" class="accordion-item__title" v-html="item?.title" />
    <div
      ref="accordionContent"
      class='accordion-item__text'
      :style="{ 'max-height': bodyHeightStyle }"
    >
    {{ item?.body }}
    </div>
  </div>
</template>

<script lang="ts">
import { ref, Ref, computed, watch, defineComponent, PropType } from 'vue'
import { accordionModeType, accordionItemType } from './accordion.type'

export default defineComponent({
  name: 'AccordionItem',
  props: {
    item: {
      type: Object as PropType<accordionItemType>
    },
    mode: {
      type: Object as PropType<accordionModeType>
    },
    index: Number,
    currentIndex: Number
  },
  emits: ['update:index'],
  setup (props, { emit }) {
    const isShow = ref(false)
    const accordionContent = ref<InstanceType<typeof HTMLDivElement>>() as Ref<InstanceType<typeof HTMLDivElement>>

    const bodyHeightStyle = computed(() => {
      if (!isShow.value) {
        return '0px'
      }

      return `${accordionContent.value?.scrollHeight || 0}px`
    })

    watch(
      () => props.mode,
      () => {
        updateCurrentMode()
      }
    )

    watch(
      () => props.currentIndex,
      () => {
        if (props.mode !== 'closable') {
          return
        }

        if (props.index !== props.currentIndex) {
          isShow.value = false
        }
      }
    )

    const openItem = () => {
      isShow.value = !isShow.value

      emit('update:index', props.index)
    }

    const updateCurrentMode = () => {
      switch (props.mode) {
        case 'opened': {
          isShow.value = true

          break
        }

        case 'closable': {
          if (props.currentIndex !== props.index) {
            isShow.value = false
          }
          break
        }

        default: isShow.value = false
      }
    }

    return { isShow, bodyHeightStyle, accordionContent, openItem }
  }
})
</script>

<style scoped  lang="scss">
.accordion-item {
  position: relative;
  padding: 10px;
  max-width: 960px;

  &__title {
    font-size: 18px;
    font-weight: bold;
    background: rgb(223, 223, 241);
    border-radius: 10px;
    padding: 15px 10px;
    text-align: left;
    cursor: pointer;
    display: flex;
    justify-content: space-between;
    align-items: center;

    @media (max-width: 640px) {
      flex-direction: column-reverse;
    }
  }

  &__text {
    margin-top: 15px;
    transition: max-height 0.3s;
    overflow: hidden;
  }
}
</style>
