<template>
  <h1 id="title">WE'RE LAUNCHING SOON</h1>
  <div id="timer">
    <TimerCard :currTime="time.d" timerType="days"/>
    <TimerCard :currTime="time.h" timerType="hours"/>
    <TimerCard :currTime="time.m" timerType="minuts"/>
    <TimerCard :currTime="time.s" timerType="seconds"/>
  </div>
  <div id="hills">
    <svg class="facebook" xmlns="http://www.w3.org/2000/svg" width="24" height="24"><path fill="#8385A9" d="M22.675 0H1.325C.593 0 0 .593 0 1.325v21.351C0 23.407.593 24 1.325 24H12.82v-9.294H9.692v-3.622h3.128V8.413c0-3.1 1.893-4.788 4.659-4.788 1.325 0 2.463.099 2.795.143v3.24l-1.918.001c-1.504 0-1.795.715-1.795 1.763v2.313h3.587l-.467 3.622h-3.12V24h6.116c.73 0 1.323-.593 1.323-1.325V1.325C24 .593 23.407 0 22.675 0z"/></svg>
    <svg class="pinterest" xmlns="http://www.w3.org/2000/svg" width="24" height="24"><path fill="#8385A9" d="M12 0C5.373 0 0 5.372 0 12c0 5.084 3.163 9.426 7.627 11.174-.105-.949-.2-2.405.042-3.441.218-.937 1.407-5.965 1.407-5.965s-.359-.719-.359-1.782c0-1.668.967-2.914 2.171-2.914 1.023 0 1.518.769 1.518 1.69 0 1.029-.655 2.568-.994 3.995-.283 1.194.599 2.169 1.777 2.169 2.133 0 3.772-2.249 3.772-5.495 0-2.873-2.064-4.882-5.012-4.882-3.414 0-5.418 2.561-5.418 5.207 0 1.031.397 2.138.893 2.738a.36.36 0 01.083.345l-.333 1.36c-.053.22-.174.267-.402.161-1.499-.698-2.436-2.889-2.436-4.649 0-3.785 2.75-7.262 7.929-7.262 4.163 0 7.398 2.967 7.398 6.931 0 4.136-2.607 7.464-6.227 7.464-1.216 0-2.359-.631-2.75-1.378l-.748 2.853c-.271 1.043-1.002 2.35-1.492 3.146C9.57 23.812 10.763 24 12 24c6.627 0 12-5.373 12-12 0-6.628-5.373-12-12-12z"/></svg>
    <svg class="instagram" xmlns="http://www.w3.org/2000/svg" width="24" height="24"><path fill="#8385A9" d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12c0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24c3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98C15.668.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 100 12.324 6.162 6.162 0 000-12.324zM12 16a4 4 0 110-8 4 4 0 010 8zm6.406-11.845a1.44 1.44 0 100 2.881 1.44 1.44 0 000-2.881z"/></svg></div>
</template>

<script>
import { reactive, ref } from '@vue/reactivity';
import TimerCard from "./components/TimerCard";
import date from 'date-and-time'
import { onMounted, watch } from '@vue/runtime-core';
export default {
  name: 'App',
  components: {
    TimerCard,
  },
  setup() {
    function Rickrolling(){
      let hasRickrolled = ref(localStorage.getItem("rickrolled"))
      
      if(!hasRickrolled.value) { localStorage.setItem("rickrolled",true) }
      return { hasRickrolled }     
    }

    const time = reactive({
      d: 0,
      h: 0,
      m: 0,
      s: 0,
    })

    const to = reactive({
      day: 24 * 3600000,
      hour: 3600000,
      minute: 60 * 1000,
      second: 1000
    })
    const day = ref( 2 * to.day )
    watch(day,() => {
      let time_left = day.value

      time.d = Math.floor(time_left / to.day)
      time_left -= time.d * to.day
      
      time.h = Math.floor(time_left / to.hour)
      time_left -= time.h * to.hour

      time.m = Math.floor(time_left / to.minute)
      time_left -= time.m * to.minute
      
      time.s = Math.floor(time_left / to.second)
    })

    const isTabVisible = ref(true)
    const pauseTime = ref(null)
    const passedTime = ref(null)
    const timeChanged = ref(false)


    onMounted(()=> {
      document.addEventListener("visibilitychange",()=>{
        isTabVisible.value = document.visibilityState == "visible"
      })
    })
    watch(isTabVisible,() => {
      if(isTabVisible.value) {
        passedTime.value = date.subtract(new Date(),pauseTime.value).toMilliseconds()
        day.value -= passedTime.value
        pauseTime.value = null
      } else {
        pauseTime.value = new Date()
      }
    })

    timeChanged.value = true
    day.value -= 1000

    const { hasRickrolled  } = Rickrolling()
    if(!hasRickrolled.value){
      day.value = 5 * to.second
    }
    setInterval(() => {
      console.log(hasRickrolled.value)
      if(!isTabVisible.value){ return }
      if(day.value <= 0) {
        day.value = 0
        if(!hasRickrolled.value){
          window.open("https://www.youtube.com/watch?v=dQw4w9WgXcQ")
          hasRickrolled.value = true
          day.value = 2 * to.day

          //this will not work most of the time but it worth a shot
        }
        return
      }
      timeChanged.value = true
      day.value -= 1000

    },1000)
  
    return { isTabVisible ,passedTime, time , day,to}
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Red+Hat+Text:wght@500;700&display=swap');
@import "./shared";
html {
  min-width: $min-width * 1px;
  overflow: hidden;
}
html , body, #app{
  width: 100%;
  height: 100%;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Red Hat Text', sans-serif;
}

#app {
  background: #201e2b;
  background-image: url("./assets/bg-stars.svg");
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 140px;
  @include media("<=tablet",">phone"){
    padding-top: applyMath(140,120,$tablet,$phone);
  }
  @include media("<=500px",">min-width"){
    padding-top: applyMath(95,70,$phone,$min-width);
  }
  @include media("<=min-width"){
    padding-top: 70px;
  }
  #title {
    font-weight: 700;
    letter-spacing: 5px;
    font-size: 30px;
    text-align: center;
    @include media("<=tablet",">phone"){
      font-size: applyMath(30,25,$tablet,$phone);
    }
    @include media("<=phone",">min-width"){
      font-size: applyMath(25,20,$phone,$min-width);
      width: applyMath(345,320,$phone,$min-width);
    }
    @include media("<=min-width"){
      font-size: 20px;
      width: 320px;    
    }
  }
  #timer {
    display: flex;
    gap: 20px;
    width: 100%;
    justify-content: center;
    padding-top: 100px;
    column-gap: 35px;
    @include media("<=tablet",">phone"){
      //padding-top: 100px;
    }
    @include media("<=phone"){
      display: grid;
      grid-template-columns: 1fr 1fr;
      width: 90%;

      column-gap: 0;
      row-gap: 25px;
      padding-top: 40px;
    }
  }
  #hills {
    width: 100%;
    height: 300px;
    background-image: url("./assets/pattern-hills.svg");
    background-repeat: no-repeat;
    background-size: 100% 80%;
    background-position: 0 100%;
    
    display: flex;
    align-items: flex-end;
    justify-content: center;
    padding-bottom:80px;
    gap: 30px;
    
    margin-top: 163px;



    .facebook,.pinterest,.instagram {
      cursor: pointer;
      &:hover {path {fill: #fb6087;}}
    }

    @include media("<=tablet",">phone"){
      background-size: 800px 158px;
      background-position: applyMath(0,-185,$tablet,$phone) 100%;
    }
    @include media("<=phone",">min-width"){
      background-size: applyMath(900,1050,$phone,$min-width) 200px;
      background-position: applyMath(-400,-650,$phone,$min-width) applyMath(90,100,$phone,$min-width);
      margin-top: applyMath(4,45,$phone,$min-width);
      padding-bottom: 30px;
    }

    @include media("<=min-width"){
      background-size: 1050px 200px;
      background-position: -650px 100px;
      margin-top: 45px;
      padding-bottom: 30px;      
    }
  }
  
}
</style>
