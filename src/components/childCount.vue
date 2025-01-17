<template>
  <li>
    <p>透過修改 defineModel 再接著取用 props 內資訊或 defineModel 的結果是異步的。</p>
  </li>
  <li>
    <slot name="count3Slot"></slot>
  </li>
  <li>
    <span>props.count3: </span>
    <span>{{ props.count3 }}</span>
  </li>
  <li>
    <span>_count3: </span>
    <span>{{ _count3 }}</span>
  </li>
  <li>
    <button @click="addDefineModelCount3">_count3++</button>
  </li>

  <hr />

  <li>
    <p>透過修改 writable computed 再接著取用 props 內資訊或 writable computed 的結果是異步的。</p>
  </li>
  <li>
    <slot name="count4Slot"></slot>
  </li>
  <li>
    <span>props.count4: </span>
    <span>{{ props.count4 }}</span>
  </li>
  <li>
    <span>_count4: </span>
    <span>{{ _count4 }}</span>
  </li>
  <li>
    <button @click="addWritableComputedCount4">_count4++</button>
  </li>
</template>
<script lang="ts" setup>
import { computed } from 'vue'

const props = defineProps<{
  count3: number
  count4: number
}>()

const emits = defineEmits<{
  changeChildCount3: []
  changeChildCount4: []
  'update:count4': [payload: number]
}>()

/**
 * 測試修改 defineModel 的數值更新。
 */

const _count3 = defineModel<number>('count3', { required: true })

const addDefineModelCount3 = () => {
  _count3.value++
  console.log(`_count3: ${_count3.value}`)
  console.log(`props.count3: ${props.count3}`)
  emits('changeChildCount3')
}

/**
 * 測試修改 writable computed 的數值更新。
 */

const _count4 = computed({
  get: () => props.count4,
  set: (value: number) => {
    emits('update:count4', value)
  },
})

const addWritableComputedCount4 = () => {
  _count4.value++
  console.log(`_count4: ${_count4.value}`)
  console.log(`props.count4: ${props.count4}`)
  emits('changeChildCount4')
}
</script>
