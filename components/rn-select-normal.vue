<template>
  <div ref="selectRef" class="rn-select">
    <button class="rn-select-btn" @click="isOpen = !isOpen">
      <slot name="label" :option="props.options[selected]"></slot>
      <i class="rn-select-arrow" :class="{ 'rotate': isOpen }"></i>
    </button>
    <div class="rn-select-option" v-if="isOpen">
      <ul>
        <li v-for="(option, key) in props.options" :key="option" @click="clickOption(key)">
          <slot name="option" :option="option"></slot>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  modelValue: {
    type: Number,
    default: 0,
  },
  options: {
    type: Array,
    default: [],
  },
})

const selected = defineModel()

const selectRef = ref(null)
const isOpen = ref(false)

const clickOption = key => {
  selected.value = key
  isOpen.value = false
}

const clickOutside = (event) => {
  if (selectRef.value && !selectRef.value.contains(event.target)) {
    isOpen.value = false
  }
}

const getWindow = element => {
  return element.ownerDocument ? element.ownerDocument.defaultView : window
}

onMounted(() => {
  getWindow(selectRef.value).addEventListener('click', clickOutside)
})

onUnmounted(() => {
  getWindow(selectRef.value).removeEventListener('click', clickOutside)
})
</script>

<style scoped>
.rn-select {
  position: relative;
}
.rn-select-btn {
  gap: 8px;
  padding: 10px;
  display: flex;
  cursor: pointer;
  border-radius: 4px;
  align-items: center;
  border: 1px solid #dddddd;
  background-color: #ffffff;
  justify-content: space-between;
}
.rn-select-btn:focus {
  outline: none;
  border-color: #00a651;
  box-shadow: 0 0 5px #00a65180;
}
.rn-select-arrow {
  padding: 3px;
  border-style: solid;
  display: inline-block;
  border-width: 0 2px 2px 0;
  transition: transform 0.2s ease;
  transform: translateY(-25%) rotate(45deg);
}
.rn-select-arrow.rotate {
  transform: rotate(-135deg);
}
.rn-select-option {
  left: 0;
  position: absolute;
  border-radius: 4px;
  top: calc(100% + 4px);
  border: 1px solid #dddddd;
  background-color: #ffffff;
  box-shadow: 0 2px 6px #0000001a;
}
.rn-select-option ul {
  margin: 0;
  padding: 10px;
  list-style: none;
}
.rn-select-option li {
  cursor: pointer;
  padding: 7px 5px;
  overflow: hidden;
  border-radius: 4px;
  white-space: nowrap;
  text-overflow: ellipsis;
  border-bottom: 1px solid #e0e0e0;
  transition: background 0.3s ease, transform 0.3s ease;
}
.rn-select-option li:hover {
  background: #e7e7e7;
  transform: translateX(5px);
}
.rn-select-option li:active {
  background: #e0e0e0;
  transform: translateX(2px);
}
.rn-select-option li:last-child {
  border-bottom: none;
}
</style>