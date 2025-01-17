<template>
  <h1>示範不同情境下異動 writable computed 與 defineModel</h1>
  <ul>
    <li>
      <p>透過修改 ref 再接著取用 computed 的結果是同步的。</p>
    </li>
    <li>
      <span>refCount1: </span>
      <span>{{ refCount1 }}</span>
    </li>
    <li>
      <span>doubleRefCount1: </span>
      <span>{{ doubleRefCount1 }}</span>
    </li>
    <li>
      <button @click="addRefCount1">refCount1++</button>
    </li>

    <hr />

    <li>
      <p>透過修改 ref 再接著取用 writable computed 的結果是同步的。</p>
    </li>
    <li>
      <p>透過修改 writable computed 再接著取用 ref 或其他 computed 的結果也是同步的。</p>
    </li>
    <li>
      <span>refCount2: </span>
      <span>{{ refCount2 }}</span>
    </li>
    <li>
      <span>writableComputedCount2: </span>
      <span>{{ writableComputedCount2 }}</span>
    </li>
    <li>
      <span>doubleRefCount2: </span>
      <span>{{ doubleRefCount2 }}</span>
    </li>
    <li>
      <span>doubleWritableComputedCount2: </span>
      <span>{{ doubleWritableComputedCount2 }}</span>
    </li>
    <li>
      <button @click="addRefCount2">refCount2++</button>
    </li>
    <li>
      <button @click="addWritableComputedCount2">writableComputedCount2++</button>
    </li>

    <hr />

    <ChildCountComponent
      v-model:count3="refCount3"
      v-model:count4="refCount4"
      @changeChildCount3="changeChildCount3Trigger"
      @changeChildCount4="changeChildCount4Trigger"
    >
      <template #count3Slot>
        <span>refCount3: </span>
        <span>{{ refCount3 }}</span>
      </template>

      <template #count4Slot>
        <span>refCount4: </span>
        <span>{{ refCount4 }}</span>
      </template>
    </ChildCountComponent>
  </ul>
  <div>
    <h2>相關連結:</h2>
    <ol>
      <li v-for="link in links" :key="link">
        <a :href="link" target="_blank" rel="noopener noreferrer">{{ link }}</a>
      </li>
    </ol>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import ChildCountComponent from './components/childCount.vue'

const refCount1 = ref(0)
const doubleRefCount1 = computed(() => refCount1.value * 2)

const addRefCount1 = () => {
  refCount1.value++
  console.log(`refCount1: ${refCount1.value}`)
  console.log(`doubleRefCount1: ${doubleRefCount1.value}`)
}

const refCount2 = ref(0)
const writableComputedCount2 = computed({
  get: () => refCount2.value,
  set: (value: number) => {
    refCount2.value = value
  },
})
const doubleRefCount2 = computed(() => refCount2.value * 2)
const doubleWritableComputedCount2 = computed(() => writableComputedCount2.value * 2)

const addRefCount2 = () => {
  refCount2.value++
  console.log(`refCount2: ${refCount2.value}`)
  console.log(`writableComputedCount2: ${writableComputedCount2.value}`)
  console.log(`doubleRefCount2: ${doubleRefCount2.value}`)
  console.log(`doubleWritableComputedCount2: ${doubleWritableComputedCount2.value}`)
}

const addWritableComputedCount2 = () => {
  writableComputedCount2.value++
  console.log(`refCount2: ${refCount2.value}`)
  console.log(`writableComputedCount2: ${writableComputedCount2.value}`)
  console.log(`doubleRefCount2: ${doubleRefCount2.value}`)
  console.log(`doubleWritableComputedCount2: ${doubleWritableComputedCount2.value}`)
}

/**
 * 測試父子元件之間的綁定數值的修改狀態
 */

const refCount3 = ref(0)
const refCount4 = ref(0)

const changeChildCount3Trigger = () => {
  console.log(`refCount3: ${refCount3.value}`)
}

const changeChildCount4Trigger = () => {
  console.log(`refCount4: ${refCount4.value}`)
}

const links = [
  'https://github.com/vuejs/core/issues/11832',
  'https://github.com/vuejs/core/issues/6130',
  //
]
</script>

<style lang="css">
#app {
  margin: 16px;
}

p,
li {
  margin: 8px 0;
}

body {
  margin: 0;
}
</style>
