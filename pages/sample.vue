<template>
  <form>
    <input
      id="r-1-1"
      type="radio"
      name="r-1">
    <input
      id="r-1-2"
      type="radio"
      name="r-1">
    <input
      id="r-1-3"
      type="radio"
      name="r-1">
    <input
      id="r-1-4"
      type="radio"
      name="r-1">

    <input
      id="r-2-1"
      type="radio"
      name="r-2">
    <input
      id="r-2-2"
      type="radio"
      name="r-2">
    <input
      id="r-2-3"
      type="radio"
      name="r-2">
    <input
      id="r-2-4"
      type="radio"
      name="r-2">

    <input
      id=""
      type="checkbox"
      name="">
    <input
      id=""
      type="checkbox"
      name="">
    <input
      id=""
      type="checkbox"
      name="">
    <input
      id=""
      type="checkbox"
      name="">

    <div class="container">
      <div class="top">
        <div class="title">
          Touch Screen
        </div>
        <div class="label-wrapper">
          <label for="r-1-1"/>
          <label for="r-1-2"/>
          <label for="r-1-3"/>
          <label for="r-1-4"/>
          <label for="r-1-1"/>
          <label for="r-1-2"/>
          <label for="r-1-3"/>
          <label for="r-1-4"/>
        </div>
      </div>
      <div class="game">
        <div class="help">
          <p>
            カードは全8種類<br>
            お手つき3回でGameOverです。
          </p>
          <div class="label-wrapper">
            <label for="r-2-1"/>
            <label for="r-2-2"/>
            <label for="r-2-3"/>
            <label for="r-2-4"/>
            <label for="r-2-1"/>
            <label for="r-2-2"/>
            <label for="r-2-3"/>
            <label for="r-2-4"/>
          </div>
        </div>
        Game
        <div class="c-1">
          <div class="m-1">1</div>
          <div class="m-2">2</div>
          <div class="m-3">3</div>
          <div class="m-4">4</div>
        </div>
        <div class="c-2">
          <div class="m-1">1</div>
          <div class="m-2">2</div>
          <div class="m-3">3</div>
          <div class="m-4">4</div>
        </div>
        <div class="cards"/>
        <div class="failed">
          <div class="failed f-1">failed1</div>
          <div class="failed f-2">failed2</div>
          <div class="failed f-3">failed3</div>
        </div>
        <div class="result">
          clear
          <input
            type="reset"
            value="">
        </div>
      </div>
    </div>
  </form>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {}
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  background: #f0f0f0;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.top,
.game,
.help,
.failed,
.result {
  background: #fff;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.game,
.failed,
.result {
  display: none;
}

.label-wrapper {
  position: absolute;
  height: 200%;
  width: 100%;
  animation: toptobottom linear 5s infinite;
  display: flex;
  flex-direction: column;
}

.label-wrapper:active {
  animation-play-state: paused;
}

.label-wrapper label {
  flex: 1;
  display: block;
  width: 100%;
}

.top label:nth-child(4n) {
  /* background: red; */
}
.top label:nth-child(4n + 1) {
  /* background: green; */
}
.top label:nth-child(4n + 2) {
  /* background: blue; */
}
.top label:nth-child(4n + 3) {
  /* background: yellow; */
}
.title {
  line-height: 100vh;
  text-align: center;
}

.game {
  /* background: #303030; */
}

.game .help {
  background: rgba(0, 0, 0, 0.6);
}

.game .help p {
  background: #fff;
  position: relative;
  top: 20px;
  left: 20px;
  right: 20px;
  bottom: 20px;
  height: calc(100% - 40px);
  width: calc(100% - 40px);
  padding: 30% 0;
}
.c-1 div,
.c-2 div {
  display: none;
}

#r-1-1:checked ~ .container .game .c-1 .m-1,
#r-1-2:checked ~ .container .game .c-1 .m-2,
#r-1-3:checked ~ .container .game .c-1 .m-3,
#r-1-4:checked ~ .container .game .c-1 .m-4,
#r-2-1:checked ~ .container .game .c-2 .m-1,
#r-2-2:checked ~ .container .game .c-2 .m-2,
#r-2-3:checked ~ .container .game .c-2 .m-3,
#r-2-4:checked ~ .container .game .c-2 .m-4 {
  display: block;
}

#r-1-1:checked ~ .container .top,
#r-1-2:checked ~ .container .top,
#r-1-3:checked ~ .container .top,
#r-1-4:checked ~ .container .top {
  display: none;
}
#r-1-1:checked ~ .container .game,
#r-1-2:checked ~ .container .game,
#r-1-3:checked ~ .container .game,
#r-1-4:checked ~ .container .game {
  display: block;
}
#r-2-1:checked ~ .container .game .help,
#r-2-2:checked ~ .container .game .help,
#r-2-3:checked ~ .container .game .help,
#r-2-4:checked ~ .container .game .help {
  display: none;
}
@keyframes toptobottom {
  0% {
    top: -100%;
  }
  100% {
    top: 0;
  }
}
</style>
