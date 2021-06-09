<template>
    <div id="scroller" class="board-containner">
        <div class="board horizontal">
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

                <div class="input-exam" v-if="item.showAns">{{ item.roma }}</div>
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
    <div class="footer">
        <div class="text">{{ corrected.length }}/{{ textLength }}</div>
        <!-- <div>{{ scrollTop }}</div> -->
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
            scrollTop: 0,
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

        /*
         document.addEventListener('focusout', (e) => {
             document.getElementById('scroller').scrollTop = this.scrollTop;
         });
         */
        document.getElementById('scroller').addEventListener('scroll', this.detectScroll);
    },
    methods: {
        detectScroll(e) {
            this.scrollTop = document.getElementById('scroller').scrollTop;
        },
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
            // 放棄
            if (item.inputValue[0] === ' ' || item.inputValue.split('').indexOf(' ') > -1) {
                item.isAnsCorrect = -1;
                item.showAns = true;
                this.focus(index + 1);
            }

            // error
            const stringLength = item.inputValue.length;
            for (let i = 0; i < stringLength; i++) {
                if (
                    item.inputValue.toLowerCase()[i] !== item.roma[i] &&
                    item.inputValue.toLowerCase()[i] !== item.roma2[i]
                ) {
                    item.isAnsCorrect = -1;
                }
            }

            if (item.inputValue === '' || !item.inputValue) {
                item.isAnsCorrect = 0;
            }

            if (item.inputValue.toLowerCase() === item.roma || item.inputValue.toLowerCase() === item.roma2) {
                item.isAnsCorrect = 1;
                if (index != this.textLength - 1) {
                    this.focus(index + 1);
                }
            }

            //force to lowercase
            event.target.value = event.target.value.toLowerCase();
        },
        focus(index) {
            this.$nextTick(() => {
                const next = 'input' + index;
                const nextRef = this.$refs[next];
                nextRef.focus();
            });
        },
        reset() {
            this.init();
        },
    },
};
</script>

<style lang="scss">
.board {
    display: flex;
    margin: 30px auto;
    flex-wrap: wrap;
    justify-content: start;
    align-content: stretch;
    align-items: stretch;
}
.board.landscape {
    height: 100%;
    flex-direction: column;
}
.board.horizontal {
    width: 100%;
    flex-direction: row;
}
.word-card {
    display: flex;
    flex-direction: column;
    /* flex-grow: 1; */
    justify-content: center;
    align-items: center;
    width: 60px;
    height: 70px;
    color: #00b9df;
    font-weight: bold;
    font-size: 30px;
    margin: 10px;
}
.word-card.correct {
    /* border: 1px solid #5bde30; */
    /* background-color: #d6ffc8; */
    .input-exam {
        background-color: #00b9df;
        color: #fff;
        border: 2px solid #00b9df;
    }
}
.word-card.wrong {
    /* border: 1px solid rgb(249 63 63);
    background-color: #e8bbbb; */
    .input-exam {
        background-color: #fd5854;
        color: #fff;
        border: 2px solid #fd5854;
    }
}
.input-exam {
    outline: none;
    padding: 0;
    box-shadow: none;
    appearance: none;
    height: 40px;
    width: 40px;
    color: #d4d4d4;
    font-size: 18px;
    text-align: center;
    border-radius: 8px;
    border: 2px solid #d4d4d4;
    line-height: 40px;
    font-weight: bold;
    caret-color: #00b9df;
    box-sizing: content-box;
    background-clip: padding-box;
    &:focus,
    &:focus-visible {
        border: 2px solid #00b9df;
        outline: none;
    }
}

.footer {
    height: 60px;
    background-color: #00b9df;

    position: fixed;
    bottom: 0;
    right: 0;
    left: 0;
    .text {
        margin-top: 20px;
        color: #fff;
    }
}

.board-containner {
    position: absolute;
    top: 140px;
    bottom: 60px;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;

    &.disable-scroll {
        position: fixed;
        left: 0;
        right: 0;
        overflow: hidden;
    }
}

@media only screen and (min-device-width: 375px) and (max-device-width: 812px) and (orientation: portrait) {
    .board-containner {
        top: 150px;
        bottom: 30px;
        .board {
            margin: 0 auto;
        }
    }
    .word-card {
        width: 55px;
    }

    .footer {
        height: 30px;
        .text {
            margin-top: 7px;
        }
    }
}

@media only screen and (min-device-width: 375px) and (max-device-width: 812px) and (orientation: landscape) {
    .board-containner {
        top: 105px;
        bottom: 30px;
        .board {
            margin: 0 auto;
        }
    }
    .word-card {
        width: 53px;
    }

    .footer {
        height: 30px;
        .text {
            margin-top: 7px;
        }
    }
}
</style>
