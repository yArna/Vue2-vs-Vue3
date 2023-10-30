<script setup>
import { nextTick, ref, onMounted } from "vue";
import Item from "./components/Item.vue";

let timeJs = ref(null);
let timeNextTick = ref(null);
let timeInitData = ref(null);
let timeMounted = ref(null);
let timeLog = ref([]);

function pushTimeLog() {
  timeLog.value.push([timeJs.value, timeNextTick.value]);
}

onMounted(() =>
  nextTick(() => {
    requestAnimationFrame(() => {
      let t0 = performance.getEntries()[0].domContentLoadedEventEnd;
      let t1 = performance.now();
      timeMounted.value = t1 - t0;
    });
  })
);

let count = 29999;
let list = Array(count)
  .fill(0)
  .map((v, i) => ({
    index: i,
    name: `name${i}`,
    state: {
      selected: false,
      highlight: false,
    },
    text: "",
  }));

{
  console.time("ref list");
  let t0 = performance.now();
  list = ref(list);
  let t1 = performance.now();
  timeInitData.value = t1 - t0;
  console.timeEnd("ref list");
}

function selectAll() {
  console.time("selectAll");
  let t0 = performance.now();
  for (const item of list.value) {
    item.state.selected = !item.state.selected;
  }
  let t1 = performance.now();
  timeJs.value = t1 - t0;
  nextTick(() => {
    requestAnimationFrame(() => {
      let t2 = performance.now();
      timeNextTick.value = t2 - t0;
      pushTimeLog();
    });
  });
  console.timeEnd("selectAll");
}

function highlightAll() {
  console.time("highlightAll");
  let t0 = performance.now();
  for (const item of list.value) {
    item.state.highlight = !item.state.highlight;
  }
  let t1 = performance.now();
  timeJs.value = t1 - t0;
  nextTick(() => {
    requestAnimationFrame(() => {
      let t2 = performance.now();
      timeNextTick.value = t2 - t0;
      pushTimeLog();
    });
  });
  console.timeEnd("highlightAll");
}

function textAll() {
  console.time("textAll");
  let t0 = performance.now();
  for (const item of list.value) {
    item.text = Math.random().toFixed(5);
  }
  let t1 = performance.now();
  timeJs.value = t1 - t0;
  nextTick(() => {
    requestAnimationFrame(() => {
      let t2 = performance.now();
      timeNextTick.value = t2 - t0;
      pushTimeLog();
    });
  });
  console.timeEnd("textAll");
}

function changeData() {
  console.time("changeData");
  let t0 = performance.now();
  for (const item of list.value) {
    item.state.highlight = Math.random() > 0.5;
    item.state.selected = Math.random() > 0.5;
    item.text = Math.random().toFixed(5);
  }
  let t1 = performance.now();
  timeJs.value = t1 - t0;
  nextTick(() => {
    requestAnimationFrame(() => {
      let t2 = performance.now();
      timeNextTick.value = t2 - t0;
      pushTimeLog();
    });
  });
  console.timeEnd("changeData");
}

function changeSort() {
  console.time("changeSort");
  let t0 = performance.now();
  for (const item of list.value) {
    item.i = Math.random();
  }
  list.value.sort((a, b) => a.i - b.i);
  let t1 = performance.now();
  timeJs.value = t1 - t0;
  nextTick(() => {
    requestAnimationFrame(() => {
      let t2 = performance.now();
      timeNextTick.value = t2 - t0;
      pushTimeLog();
    });
  });
  console.timeEnd("changeSort");
}
</script>

<template>
  <div>
    <div class="wrapper">
      <Item v-for="item in list" :item="item" :key="item.index" />
    </div>

    <div class="tools">
      <div class="div">
        Vue2 <b>{{ count }}</b
        >items
        <span
          >timeInitData: <b>{{ timeInitData.toFixed(2) }}ms</b></span
        >
        <span
          >timeMounted: <b>{{ timeMounted?.toFixed(2) }}ms</b></span
        >
      </div>
      <div class="div actions">
        <button @click="selectAll()">selectAll</button>
        <button @click="highlightAll()">highlightAll</button>
        <button @click="textAll()">textAll</button>
        <button @click="changeData()">changeData</button>
        <button @click="changeSort()">changeSort</button>

        <div v-if="timeJs && timeNextTick" class="action-time">
          <div class="timeJs">
            <div class="lable">Js</div>
            {{ timeJs.toFixed(2) }}ms
          </div>
          <div class="timeNextTick">
            <div class="lable">NextTick</div>
            {{ timeNextTick.toFixed(2) }}ms
          </div>
        </div>
      </div>
    </div>

    <div class="timeLog">
      <textarea :value="JSON.stringify(timeLog)"> </textarea>
    </div>
  </div>
</template>

<style>
body {
  margin: 0;
  padding: 0;
  background: #f8fdff;
  padding-top: 60px;
}

.tools {
  position: fixed;
  flex-direction: column;
  top: 0;
  left: 0;
  width: 100%;
  height: 73px;
  display: flex;
  place-items: flex-start;
  padding: 12px 30px;

  background: #e7eef0;
  border-bottom: 1px solid #cddbe1;
  box-shadow: 0 0 2px #0922442b;
}

.tools > div {
  display: flex;
  gap: 12px;
  place-items: center;
  place-content: flex-start;
}

.wrapper {
  display: flex;
  flex-wrap: wrap;
  gap: 4px;
  width: 920px;
  margin: 0 auto;
}

.actions {
  height: 58px;
}

.action-time {
  display: flex;
  flex-direction: column;
  background: #40476c;
  color: #ebebfa;
  padding: 6px 12px;
  border-radius: 3px;
}
.action-time > div {
  display: flex;
  font-size: 12px;
  place-content: space-between;
}
.action-time > div .lable {
  width: 70px;
}

.timeLog {
  position: fixed;
  bottom: 0;
  right: 0;
  height: auto;
  width: auto;
  background: #e7eef0;
  padding: 4px;
}
</style>
