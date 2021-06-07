<template>
    <!-- <Exam :examType="'hiragana'" /> -->

    <div class="header">
        <div class="desc">輸入答案即可開始測驗，按空白鍵顯示答案呦!</div>

        <div class="logo-wrapper">
            <div class="logo"></div>
        </div>
    </div>

    <div class="toolbar">
        <div class="toolbar__tab">
            <input type="radio" id="hiragana" value="hiragana" v-model="pickedType" />
            <label for="hiragana">平假名</label>
            <!-- <label for="hiragana">hiragana</label> -->
        </div>

        <div class="toolbar__tab">
            <input type="radio" id="katakana" value="katakana" v-model="pickedType" />
            <label for="katakana">片假名</label>
            <!-- <label for="katakana">katakana</label> -->
        </div>

        <div class="toolbar__divide-line"></div>

        <button class="toolbar__tab--button" @click="reset">重新開始</button>
    </div>

    <Exam ref="exam" :examType="pickedType" />
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
    methods: {
        reset() {
            this.$refs.exam.reset();
        },
    },
};
</script>

<style lang="scss">
.header {
    height: 60px;
    background-color: #00b9df;
    box-shadow: 3px 3px 7px 6px rgb(195 195 195 / 46%);
    position: relative;
    .desc {
        font-size: 16px;
        color: #fff;
        position: absolute;
        right: 200px;
        top: 20px;
    }
    .logo-wrapper {
        position: absolute;
        top: 0px;
        right: 80px;

        .logo {
            background-image: url('./assets/logo_w.png');
            width: 100px;
            height: 100px;
            background-color: #00b9df;
            border-radius: 50%;
            background-size: 70% 70%;
            background-position: center center;
            background-repeat: no-repeat;
        }
    }
}

.toolbar {
    margin-top: 20px;
    text-align: left;
    padding: 10px 100px;
    .toolbar__tab--button {
        padding: 0px 20px;
        cursor: pointer;
        font-weight: bold;

        border: solid 2px #d4d4d4;

        border-radius: 6px;
        color: #fff;
        background-color: #c5c5c5;
        line-height: 36px;
        height: 36px;
        vertical-align: top;
    }
    .toolbar__divide-line {
        line-height: 36px;
        height: 36px;
        display: inline-block;
        border-right: dotted 2px #d4d4d4;
        vertical-align: top;
        margin-right: 10px;
    }
    .toolbar__tab {
        display: inline-block;
        margin-right: 10px;
        vertical-align: top;

        input {
            position: absolute;
            width: 1px;
            height: 1px;
            padding: 0;
            margin: -1px;
            overflow: hidden;
            clip: rect(0, 0, 0, 0);
            white-space: nowrap;
            border-width: 0;
            &:checked + label {
                color: #fff;
                background-color: #00b9df;
                border: solid 2px #00b9df;
            }
            &:not(:checked) + label {
                box-shadow: none;
            }
        }
        label {
            display: inline-block;
            line-height: 32px;
            height: 32px;
            padding: 0px 20px;
            cursor: pointer;
            font-weight: bold;
            /* box-shadow: 0 1px 3px 0 rgb(0 0 0 / 10%), 0 1px 2px 0 rgb(0 0 0 / 6%); */
            background-color: transparent;
            border: solid 2px #d4d4d4;
            color: #d4d4d4;
            border-radius: 6px;
        }
    }
}
</style>
