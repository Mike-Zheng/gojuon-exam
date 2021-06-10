<template>
    <!-- <Exam :examType="'hiragana'" /> -->
    <transition name="fade">
        <Splash v-if="isLoading" marginTop="50%"></Splash>
    </transition>

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
import Splash from './components/Splash.vue';
import Exam from './components/Exam.vue';
export default {
    name: 'App',
    components: {
        // Hiragana,
        Splash,
        Exam,
    },
    data() {
        return {
            isLoading: true,
            pickedType: 'hiragana',
        };
    },
    mounted() {
        setTimeout(() => {
            this.isLoading = false;
        }, 2000);
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
    transition: all 0.2s ease;
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
            background-size: 100% 100%;
            background-position: -3px center;
            background-repeat: no-repeat;
        }
    }
}

.toolbar {
    margin-top: 20px;
    text-align: left;
    padding: 10px 100px;
    .toolbar__tab--button {
        transition: all 0.2s ease;
        padding: 0px 20px;
        cursor: pointer;
        font-weight: bold;

        border: solid 2px #c5c5c5;

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
        transition: all 0.2s ease;

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

/* Portrait */
@media only screen and (min-device-width: 375px) and (max-device-width: 812px) and (orientation: portrait) {
    .header {
        .desc {
            width: 180px;
            word-break: break-all;
            text-align: left;
            right: 90px;
            top: 16px;
        }
        .logo-wrapper {
            right: 0px;

            .logo {
                width: 90px;
                height: 90px;
            }
        }
    }

    .toolbar {
        padding: 0 20px;
        margin-top: 20px;
        .toolbar__tab--button {
            line-height: 38px;
            height: 40px;
            padding: 0 10px;
        }
        .toolbar__divide-line {
            line-height: 36px;
            height: 36px;
        }
        .toolbar__tab {
            label {
                line-height: 36px;
                height: 36px;
                padding: 0 10px;
            }
        }
    }
}

/* Portrait */
@media only screen and (min-device-width: 375px) and (max-device-width: 812px) and (orientation: landscape) {
    .header {
        .desc {
            width: 180px;
            word-break: break-all;
            text-align: left;
            right: 90px;
            top: 16px;
        }
        .logo-wrapper {
            right: 0px;

            .logo {
                width: 90px;
                height: 90px;
            }
        }
    }

    .toolbar {
        padding: 0 20px;
        margin-top: 10px;
        .toolbar__tab--button {
            line-height: 26px;
            height: 30px;
            padding: 0 10px;
        }
        .toolbar__divide-line {
            line-height: 26px;
            height: 26px;
        }
        .toolbar__tab {
            label {
                line-height: 26px;
                height: 26px;
                padding: 0 10px;
            }
        }
    }
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to
/* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
}
</style>
