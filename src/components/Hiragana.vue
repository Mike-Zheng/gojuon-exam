<template>
  <div class="board landscape">
    <div class="word-card" v-for="(item, index) in gojuon" :key="index">
      {{ item.hira_unicode }}
      {{ item.roma }}
    </div>
  </div>
</template>

<script>
import { basic } from "../gojuon.js";
export default {
  name: "Hiragana",
  props: {
    msg: String,
  },
  data() {
    return {
      gojuon: basic,
    };
  },
};
</script>

<style>
.board {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-content: stretch;
  align-items: stretch;
}
.board.landscape {
  height: 600px;
  flex-direction: column;
}
.board.horizontal {
  width: 600px;
  flex-direction: row;
}
.word-card {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  justify-content: center;
  align-items: center;
  font-weight: 300;
  width: 120px;
  height: 120px;
  border: 1px solid #ccc;
  margin: -1px;
}
</style>