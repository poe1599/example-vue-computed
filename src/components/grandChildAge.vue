<template>
  <ul>
    <li>
      <span>grandChildAge:</span>
      <span>{{ props.grandChildAge }}</span>
    </li>
    <li>
      <span>_grandChildAge:</span>
      <span>{{ _grandChildAge }}</span>
    </li>
    <li>
      <button @click="addGrandChildAge">_grandChildAge++</button>
    </li>
  </ul>
</template>
<script lang="ts" setup>
const props = defineProps<{
  grandChildAge: number
}>()

const emits = defineEmits<{
  changeGrandChildAge: []
}>()

const _grandChildAge = defineModel<number>('grandChildAge', { required: true })

const addGrandChildAge = () => {
  _grandChildAge.value++
  console.log(`_grandChildAge: ${_grandChildAge.value}`)
  console.log(`props.grandChildAge: ${props.grandChildAge}`)
  emits('changeGrandChildAge')
}
</script>
