<template>
    <div style="margin-top: 20px">{{ corrected.length }}/{{ textLength }}</div>
    <div class="board-container">
        <div class="board">
            <div
                class="word-card"
                v-for="(item, index) in textList"
                :key="index"
                :class="{
                    correct: item.isAnsCorrect == 1,
                    wrong: item.isAnsCorrect == -1,
                }"
            >
                <span v-if="examType == 'hiragana'">{{ item.hira_unicode }}</span>
                <span v-if="examType == 'katakana'">{{ item.kata_unicode }}</span>

                <span v-if="item.showAns">{{ item.roma }}</span>
                <input
                    v-else
                    :ref="'input' + index"
                    v-model="item.inputValue"
                    class="input-exam"
                    @input="validAns($event, item, index)"
                />
            </div>
        </div>
    </div>
</template>

<script>
import { basic } from '../gojuon.js';
export default {
    name: 'Exam',
    data() {
        return {
            gojuon: basic,
            textList: [],
            textLength: 100,
        };
    },
    props: {
        examType: {
            type: String,
            default: 'hiragana',
        },
    },
    watch: {
        examType: function (newValue, rawValue) {
            if (newValue !== rawValue) {
                this.init();
            }
        },
    },
    computed: {
        corrected() {
            return this.textList.filter((_) => _.isAnsCorrect === 1);
        },
    },
    mounted() {
        this.init();
    },
    methods: {
        init() {
            this.textList = [];

            while (this.textList.length < this.textLength) {
                const index = this.getRandomInt(0, 50);
                if (this.gojuon[index].roma !== '') {
                    this.textList.push({
                        ...this.gojuon[index],
                        ...{ inputValue: '', isAnsCorrect: 0, showAns: false },
                    });
                }
            }

            this.$nextTick(() => {
                this.focus(0);
            });
        },
        getRandomInt(min, max) {
            min = Math.ceil(min);
            max = Math.floor(max);
            return Math.floor(Math.random() * (max - min + 1)) + min;
        },
        validAns(event, item, index) {
            //force lowercase
            event.target.value = event.target.value.toLowerCase();

            
            // 放棄
            if (item.inputValue[0] === ' ' || item.inputValue.split('').indexOf(' ') > -1) {
                item.isAnsCorrect = -1;
                item.showAns = true;
                this.focus(index + 1);
            }

            // error
            const stringLength = item.inputValue.length;
            for (let i = 0; i < stringLength; i++) {
                if (item.inputValue[i].toLowerCase() !== item.roma[i]) {
                    item.isAnsCorrect = -1;
                }
            }

            if (item.inputValue === '' || !item.inputValue) {
                item.isAnsCorrect = 0;
            }

            if (item.inputValue.toLowerCase() === item.roma) {
                item.isAnsCorrect = 1;
                if (index != this.textLength - 1) {
                    this.focus(index + 1);
                }
            }
        },
        focus(index) {
            this.$nextTick(() => {
                const next = 'input' + index;
                const nextRef = this.$refs[next];
                nextRef.focus();
            });
        },
    },
};
</script>

<style>
.board-container {
    margin: 50px 100px;
}
.board {
    display: flex;
    width: 100%;
    flex-wrap: wrap;
    justify-content: start;
    align-content: stretch;
    align-items: stretch;
}

.word-card {
    display: flex;
    flex-direction: column;
    /* flex-grow: 1; */
    justify-content: center;
    align-items: center;
    width: 60px;
    height: 60px;
    border: 1px dotted rgb(246 246 246);
    margin: -1px;
    font-size: 18px;
}
.word-card.correct {
    border: 1px solid #5bde30;
    background-color: #d6ffc8;
}
.word-card.wrong {
    border: 1px solid rgb(249 63 63);
    background-color: #e8bbbb;
}
.input-exam {
    width: 50px;
}
</style>
