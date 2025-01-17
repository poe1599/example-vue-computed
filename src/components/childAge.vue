<template>
  <ul>
    <li>
      <span>childAge</span>
      <span>{{ props.childAge }}</span>
    </li>
    <li>
      <span>_childAge</span>
      <span>{{ _childAge }}</span>
    </li>
    <li>
      <button @click="addChildAge">_childAge++</button>
    </li>
    <li>
      <GrandChildAge
        v-model:grand-child-age="_childAge"
        @change-grand-child-age="changeGrandChildAgeTrigger"
      ></GrandChildAge>
    </li>
  </ul>
</template>
<script lang="ts" setup>
import GrandChildAge from './grandChildAge.vue'

const props = defineProps<{
  childAge: number
}>()

const emits = defineEmits<{
  changeChildAge: []
}>()

const _childAge = defineModel<number>('childAge', { required: true })

const addChildAge = () => {
  _childAge.value++
  console.log(`_childAge: ${_childAge.value}`)
  console.log(`props.childAge: ${props.childAge}`)
  emits('changeChildAge')
}

const changeGrandChildAgeTrigger = () => {
  console.log(`_childAge: ${_childAge.value}`)
  console.log(`props.childAge: ${props.childAge}`)
  emits('changeChildAge')
}
</script>
