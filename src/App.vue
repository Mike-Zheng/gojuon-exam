<template>
    <!-- <Exam :examType="'hiragana'" /> -->
    <div class="main-app">

        <div class="tabs">
            <div>
                <input class="tab-radio" type="radio" id="hiragana" value="hiragana" v-model="pickedType" />
                <label class="tab-radio-label" for="hiragana">hiragana</label>
            </div>
            <div>
                <input class="tab-radio" type="radio" id="katakana" value="katakana" v-model="pickedType" />
                <label class="tab-radio-label" for="katakana">katakana</label>
            </div>
        </div>
        <Exam :examType="pickedType" />

        
    </div>
</template>

<script>
// import Hiragana from "./components/Hiragana.vue";
import Exam from './components/Exam.vue';
export default {
    name: 'App',
    components: {
        // Hiragana,
        Exam,
    },
    data() {
        return {
            pickedType: 'hiragana',
        };
    },
};
</script>

<style scoped>
.main-app {
    background-color: #f5f7f8;
    height: 100vh;
    width: 100vw;
    overflow: hidden;
    border-radius: 5px;
}

input.tab-radio[type='radio']:checked + label {
    font-weight: bold;
    color: #333;
}

input.tab-radio[type='radio'] {
    position: absolute;
    visibility: hidden;
}

.tab-radio-label {
    position: relative;
    font-weight: 300;
    font-size: 14px;
    margin: 10px auto;
    height: 30px;
    z-index: 9;
    cursor: pointer;
    -webkit-transition: all 0.25s linear;
    color: #ccc;
}

.tabs {
    display: flex;
    margin: 10px 10%;
    background-color: #fff;
    height: 40px;
    border-radius: 5px;
    align-items: center;
    justify-content: space-around;
}
</style>
