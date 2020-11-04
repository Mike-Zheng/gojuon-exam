<template>
  <!-- <Exam :examType="'hiragana'" /> -->
  <Exam :examType="pickedType" />

  <input type="radio" id="hiragana" value="hiragana" v-model="pickedType" />
  <label for="hiragana">hiragana</label>

  <input type="radio" id="katakana" value="katakana" v-model="pickedType" />
  <label for="katakana">katakana</label>
  <br />
</template>

<script>
import Hiragana from "./components/Hiragana.vue";
import Exam from "./components/Exam.vue";
export default {
  name: "App",
  components: {
    Hiragana,
    Exam,
  },
  data() {
    return {
      pickedType: "hiragana",
    };
  },
};
</script>
