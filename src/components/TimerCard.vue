optional easing

<template>
    <div class="TimerCard-comp" >
        <div class="container">
            <div ref="el_front" :class="['front-1']">
                <img class="img-top" src="../assets/Top.png" alt="top">
                <span> {{ parsedNumber(currTime + 1 )  }} </span>
            </div>
            <div class="front-2">
                <img class="img-top" src="../assets/Top.png" alt="top">
                <span>{{ parsedNumber(currTime )  }}</span>
            </div>
            <div ref="el_back" :class="['back-1']">
                <img class="img-bottom" src="../assets/Bottom.png" alt="top">
                <span>{{ parsedNumber(currTime ) }}</span>
            </div>
            <div class="back-2">
                <img class="img-bottom" src="../assets/Bottom.png" alt="top">
                <span>{{ parsedNumber(currTime + 1  )  }}</span>
            </div>
        </div>
        <p>{{ timerType.toUpperCase() }}</p>
    </div>
</template>

<script>
import { ref, onUpdated, onMounted} from 'vue'
import anime from 'animejs/lib/anime.es.js';
export default {
    name: "TimerCard",
    props: {
        currTime: Number,
        timerType: String,
    },
    setup(){
        const el_front = ref(null)
        const el_back = ref(null)
        const playAnimation = () => {
            anime({
                targets: el_front.value,
                duration: 900,
                rotateX: [0,-180],
                easing: 'linear',
            })
            anime({
                targets: el_back.value,
                duration: 900,
                rotateX: [180,0],
                easing: 'linear',
            })
        }
        
        onMounted(() => {playAnimation()})
        onUpdated(()=>{playAnimation()})
        const parsedNumber = (num) => {
            if(num < 10 && num >= 0){
                return `0${num}`
            } else if (num >= 60) {
                return `00`
            }
            return num
        }
        return {  parsedNumber ,el_front ,el_back}
    }
}
</script>
<style lang="scss">
@import "../shared";

$text-color: #fb6087;
$darker-text-color: #d55272;
$offset-from-top: 1px;


.TimerCard-comp {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
    P {
        color: #7a768d;
        letter-spacing: 5px;
        font-weight: 700;
        @include media("<=tablet",">phone"){
            font-size: applyMath(16,10,$tablet,$phone);
        }
        @include media("<=phone"){
            font-size: applyMath(13,12,$phone,$min-width);
        }
    }
}
.container {
    width: 160px;
    height: 190px;
    position: relative;
    perspective: 1000px;
    transform-style: preserve-3d;
    @include media("<=tablet",">phone"){
        width: applyMath(160,90,$tablet,$phone);
        height: applyMath(190,130,$tablet,$phone);
    }
    @include media("<=phone"){
        width: applyMath(120,120,$phone,$min-width);
        height: applyMath(150,150,$phone,$min-width);    
    }
    span {
        line-height: 0;
        position: relative;
        font-weight: 700;

        font-size: 100px;
        @include media("<=tablet",">phone"){
            font-size: applyMath(100,52,$tablet,$phone);
        }
        @include media("<=phone"){
            font-size: applyMath(75,70,$phone,$min-width);
        }
    }
    img {
        position: absolute;
        width: 100%;
        height: 100%;
    }
    .front-1 , .back-1,
    .front-2,.back-2 {
        position: absolute;
        overflow: hidden;
        backface-visibility: hidden;

        display: flex;
        flex-direction: column;
        align-items: center;

        width: 100%;
        height: 88px;
        @include media("<=tablet",">phone"){
            height: applyMath(88,55,$tablet,$phone);
        }
        @include media("<=phone"){
            height: applyMath(70,60,$phone,$min-width);
        }

    }

    .front-2,.front-1 {
        width: 100%;
        transform-origin: 0 100%;
        color:$darker-text-color;
        
        span { top: 100%; }


    }
    .back-2,.back-1 {
        transform-origin: 0 0;       
        color: $text-color;
        margin-top: $offset-from-top;

        top: 88px + $offset-from-top;
        @include media("<=tablet",">phone"){
           top: applyMath(88,55,$tablet,$phone);
        }
        @include media("<=phone"){
            top: applyMath(70,60,$phone,$min-width);
        }
    }
    .back-2 {
        border-bottom: solid #1A1A24 12px;
        height: calc(100% / 2 + 5px);
        border-radius: 10px;
    
        @include media("<=tablet",">phone"){
            border-bottom: solid #1A1A24 10px;
            height: applyMath(98,65,$tablet,$phone);
        }
        @include media("<=phone"){
            border-bottom: solid #1A1A24 10px;
            height: applyMath(85,75,$phone,$min-width);        
        }
    }

    .front-1 , .back-1 { 
        transform: rotateX(0deg);
        z-index: 1;
    }
    .back-1 { transform: rotateX(180deg); }
}

</style>