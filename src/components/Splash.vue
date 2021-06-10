<template>
    <div class="loadEffect">
        <!-- <div class="ring-1" :style="{marginTop: marginTop? marginTop : '-50%'}"></div> -->
        <div class="loading">
            <img src="../assets/logo_w.png" class="logo pulse" />
        </div>
    </div>
</template>
<script>
export default {
    name: 'Loading',
    props: ['marginTop'],
};
</script>
<style scoped lang="scss">
.loadEffect {
    z-index: 1000;
    position: fixed;
    top: 0px;
    bottom: 0;
    right: 0;
    left: 0;
    bottom: 0;
    background-color: rgba(43, 49, 52, 0.78);
    /*transform: scale(.5)*/
}

.ring-1 {
    width: 50px;
    height: 50px;
    margin: 0 auto;
    padding: 10px;
    /*border: 7px dashed #4b9cdb;*/
    border: 7px dashed #ff6700;
    border-radius: 100%;
    position: fixed;
    left: calc(50% - 40px);
    top: calc(50% - 180px);
}

.ring-1 {
    animation: loading 1.5s 0.3s cubic-bezier(0.17, 0.37, 0.43, 0.67) infinite;
}

@keyframes loading {
    0% {
        transform: rotate(0deg);
    }
    50% {
        transform: rotate(180deg);
    }
    100% {
        transform: rotate(360deg);
    }
}

@keyframes pulse {
    0% {
        /*background-color: #369dd8;*/
        max-width: 100px;
    }
    50% {
        /*background-color: #51cfef;*/
        margin-top: -20px;
    }
    100% {
        /*background-color: #369dd8;*/
        max-width: 100px;
    }
}

.loading {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: #00b9df;
    z-index: 100;
    opacity: 1;
    transition: all 300ms ease;
}

.loading .pulse {
    animation: pulse 2s infinite;
}

.loading .logo {
    left: 50%;
    position: absolute;
    top: 50%;
    transform: translate(-50%, -50%);
}
</style>
