<template>
  <div>
    <h1>Rn-Select Test</h1>
    <div class="wrapper">
      <rn-select-normal v-model="selected" :options="items">
        <template v-if="selected == -1" #label>아이템을 선택하세요.</template>
        <template v-else #label="{ option }">{{ option.label }}</template>
        <template #option="{ option }">{{ option.label }}</template>
      </rn-select-normal>
    </div>
  </div>
</template>

<script setup>

const selected = ref(-1)

const items = ref([
  { label: 'Item 1', value: 1 },
  { label: 'Item 2', value: 2 },
  { label: 'Item 3', value: 3 },
])
</script>

<style>

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  padding: 20px;
}

.wrapper {
  width: 200px;
  height: 100px;
  padding: 10px;
  background-color: #ececec;
}
</style>