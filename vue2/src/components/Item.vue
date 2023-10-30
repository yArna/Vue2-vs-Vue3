<script setup>
const props = defineProps(["item"]);
</script>

<template>
  <div class="item" :class="{ highlight: item.state.highlight }">
    <div class="name">Item-{{ item?.index }}</div>

    <label class="selected">
      selected:
      <input type="checkbox" v-model="item.state.selected" />
    </label>

    <label class="selected">
      highlight:
      <input type="checkbox" v-model="item.state.highlight" />
    </label>

    <div class="text">
      {{ item.text }}
    </div>
  </div>
</template>

<style scoped>
.item {
  display: flex;
  flex-direction: column;
  width: 200px;
  height: 100px;
  padding: 12px;
  border-radius: 3px;
  border: 1px solid #2f5ea6;
  background: #f8fdff;
  color: #062d67;
  user-select: none;
  font-size: 12px;
}

.item .name {
  font-family: sans-serif;
  font-weight: bold;
  font-size: 13px;
}

.item.highlight {
  background: yellow;
}
</style>
