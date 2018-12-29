<template>
  <form>
    <input
      id="reset"
      type="reset"
      value="">

    <input
      v-for="r in randomCount"
      :key="r"
      :id="`radio-1-${r}`"
      name="radio-1"
      type="radio">

    <input
      v-for="r in randomCount"
      :key="r"
      :id="`radio-2-${r}`"
      name="radio-2"
      type="radio">

    <input
      v-for="p in phaseCount"
      :key="p"
      :checked="p==1"
      :id="`p-${p}`"
      name="p"
      type="radio">

    <input
      v-for="m in markCount"
      :key="m"
      :id="`check-${m}-correct`"
      type="checkbox">

    <template
      v-for="c in cardCount">
      <template
        v-for="p in phaseCount">
        <input
          :key="c,p"
          :class="[`c-${c}`, `p-${p}`]"
          :id="`check-${c}-${p}-wrong`"
          type="checkbox">

        <input
          v-for="m in markCount"
          :key="c,m,p"
          :class="[`c-${c}`, `m-${m}`, `p-${p}`]"
          :id="`check-${c}-${m}-${p}-open`"
          type="checkbox">
      </template>
    </template>

    <div class="container">
      <div class="page top">
        <div class="title">
          Touch Screen
        </div>
        <div class="label-wrapper">
          <label
            v-for="r in randomCount"
            :key="r"
            :for="`radio-1-${r}`" />
          <label
            v-for="r in randomCount"
            :key="r"
            :for="`radio-1-${r}`" />
        </div>
      </div>
      <div class="page game">
        <div class="dialog help">
          <div class="dialog-content">
            <p>
              カードは全8種類<br>
              お手つき5回でゲームオーバーです。
            </p>
          </div>
          <div class="label-wrapper">
            <label
              v-for="r in randomCount"
              :key="r"
              :for="`radio-2-${r}`" />
            <label
              v-for="r in randomCount"
              :key="r"
              :for="`radio-2-${r}`" />
          </div>
        </div>

        <div class="dialog failed">
          <label
            for="p-2"
            class="f-1"/>
          <label
            for="p-3"
            class="f-2"/>
          <label
            for="p-4"
            class="f-3"/>
          <label
            for="p-5"
            class="f-4"/>
          <label
            for="reset"
            class="f-5"/>
          <div class="dialog-content">
            <div class="f-1">
              おてつき1回目。残り4回。
            </div>
            <div class="f-2">
              おてつき2回目。残り3回。
            </div>
            <div class="f-3">
              おてつき3回目。残り2回。
            </div>
            <div class="f-4">
              おてつき4回目。残り1回。
            </div>
            <div class="f-5">
              ゲームオーバー
            </div>
          </div>
        </div>

        <div class="dialog result">
          <label for="reset"/>
          <div class="dialog-content">
            Complete!
          </div>
        </div>

        <div class="cards">
          <template
            v-for="c in cardCount">
            <div
              :key="c"
              :class="['card', `c-${c}`]">

              <template
                v-for="m in markCount">
                <label
                  v-for="p in phaseCount"
                  :key="c,m,p"
                  :class="['open', `m-${m}`, `p-${p}`]"
                  :for="`check-${c}-${m}-${p}-open`" />
              </template>

              <label
                v-for="p in phaseCount"
                :key="c,p"
                :class="['wrong', `p-${p}`]"
                :for="`check-${c}-${p}-wrong`" />

              <template
                v-for="m in markCount">
                <label
                  :key="c,m"
                  :class="['correct', `m-${m}`]"
                  :for="`check-${m}-correct`" />
              </template>

              <div class="front">
                <span
                  v-for="m in markCount"
                  :key="m"
                  :class="['face', `m-${m}`]">{{ mark[m-1] }}</span>
              </div>
              <div class="back"/>
            </div>
          </template>
        </div>
      </div>
    </div>

  </form>
</template>

<script>
export default {
  components: {},
  data: function() {
    return {
      randomCount: 8,
      phaseCount: 4,
      markCount: 8,
      mark: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
    }
  },
  computed: {
    cardCount: function() {
      return this.markCount * 2
    }
  }
}
</script>

<style lang="scss">
input {
  display: none;
}

.container {
  height: 100vh;
  overflow: hidden;
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

  .page {
    background: #fff;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }
}

.cards {
  height: 480px;
  left: 50%;
  margin: calc((100vh - 480px) / 2) auto;
  position: absolute;
  top: 0;
  transform: translateX(-50%);
  width: 320px;

  .card {
    border: 1px solid black;
    border-radius: 4px;
    width: 60px;
    height: 100px;
    line-height: 100px;
    margin: 10px;
    display: inline-block;
    text-align: center;
    vertical-align: middle;
    position: relative;
    transition: transform 1s;
    transform-style: preserve-3d;

    .front {
      border-radius: 4px;
      background: #fff;
      position: absolute;
      width: 100%;
      height: 100%;
      transform: rotateY(180deg) translateZ(1px);

      .face {
        display: none;
      }
    }

    .back {
      border-radius: 4px;
      background: #abc;
      position: absolute;
      width: 100%;
      height: 100%;
      transform: rotateY(0);
    }

    label {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 1;
    }
  }
}

.top {
  .title {
    line-height: 100vh;
    text-align: center;
    animation: flashing 1s alternate infinite;
  }
}

.game {
  display: none;

  .card {
    label,
    .front span {
      display: none;
    }
  }

  .help,
  .failed,
  .result {
    display: none;
  }

  .help {
    .dialog-content {
      padding: 30px;
    }
  }

  .failed,
  .result {
    animation: flashing 1s 1s;
    animation-fill-mode: forwards;
    opacity: 0;

    label {
      animation: fill 1s 2s;
      animation-fill-mode: forwards;
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 0;
      z-index: 1;
    }

    .dialog-content {
      padding: 30px;
    }
  }
}

.label-wrapper {
  position: absolute;
  height: 200%;
  width: 100%;
  top: 0;
  left: 0;
  animation: toptobottom 5s steps(10) infinite;
  display: flex;
  flex-direction: column;

  &:active {
    animation-play-state: paused;
  }

  label {
    flex: 1;
    display: block;
    width: 100%;
  }
}

.dialog {
  background: rgba(0, 0, 0, 0.6);
  height: 100%;
  position: relative;
  width: 100%;
  z-index: 10;

  &-content {
    background: #fff;
    left: 50%;
    position: absolute;
    transform: translateX(-50%) translateY(-50%);
    top: 50%;
  }
}

// 全てのカードを開いた
#check-1-correct:checked
  ~ #check-2-correct:checked
  ~ #check-3-correct:checked
  ~ #check-4-correct:checked
  ~ #check-5-correct:checked
  ~ #check-6-correct:checked
  ~ #check-7-correct:checked
  ~ #check-8-correct:checked
  ~ .container
  .result {
  display: block;
}

// 画面遷移
[name='radio-1']:checked ~ .container .top {
  display: none;
}
[name='radio-1']:checked ~ .container .game,
[name='radio-1']:checked ~ .container .game .help {
  display: block;
}
[name='radio-2']:checked ~ .container .game .help {
  display: none;
}
[name='radio-2']:checked ~ .container .label-wrapper {
  animation-play-state: paused;
}

// 対応しないlabelを非表示
#p-1:checked ~ .container .card label.p-2,
#p-1:checked ~ .container .card label.p-3,
#p-1:checked ~ .container .card label.p-4,
#p-1:checked ~ .container .card label.p-5,
#p-2:checked ~ .container .card label.p-1,
#p-2:checked ~ .container .card label.p-3,
#p-2:checked ~ .container .card label.p-4,
#p-2:checked ~ .container .card label.p-5,
#p-3:checked ~ .container .card label.p-1,
#p-3:checked ~ .container .card label.p-2,
#p-3:checked ~ .container .card label.p-4,
#p-3:checked ~ .container .card label.p-5,
#p-4:checked ~ .container .card label.p-1,
#p-4:checked ~ .container .card label.p-2,
#p-4:checked ~ .container .card label.p-3,
#p-4:checked ~ .container .card label.p-5,
#p-5:checked ~ .container .card label.p-1,
#p-5:checked ~ .container .card label.p-2,
#p-5:checked ~ .container .card label.p-3,
#p-5:checked ~ .container .card label.p-4 {
  display: none !important;
}

// お手付き表示切り替え
.f-1,
.f-2,
.f-3,
.f-4,
.f-5 {
  display: none;
}
#p-1:checked ~ [id$='wrong'].p-1:checked ~ .container .game .failed,
#p-1:checked ~ [id$='wrong'].p-1:checked ~ .container .game .failed .f-1,
#p-2:checked ~ [id$='wrong'].p-2:checked ~ .container .game .failed,
#p-2:checked ~ [id$='wrong'].p-2:checked ~ .container .game .failed .f-2,
#p-3:checked ~ [id$='wrong'].p-3:checked ~ .container .game .failed,
#p-3:checked ~ [id$='wrong'].p-3:checked ~ .container .game .failed .f-3,
#p-4:checked ~ [id$='wrong'].p-4:checked ~ .container .game .failed,
#p-4:checked ~ [id$='wrong'].p-4:checked ~ .container .game .failed .f-4,
#p-5:checked ~ [id$='wrong'].p-5:checked ~ .container .game .failed,
#p-5:checked ~ [id$='wrong'].p-5:checked ~ .container .game .failed .f-5 {
  display: block;
}

@for $c from 1 through 16 {
  @for $m from 1 through 8 {
    @for $p from 1 through 4 {
      // カードをめくる
      #p-#{$p}:checked
        ~ #check-#{$c}-#{$m}-#{$p}-open:checked
        ~ .container
        .game
        .cards
        .c-#{$c} {
        transform: rotateY(180deg) rotateZ(-3deg);
      }

      #p-#{$p}:checked
        ~ #check-#{$c}-#{$m}-#{$p}-open:checked
        ~ .container
        .game
        .cards
        .card.c-#{$c}
        label {
        display: none;
      }

      // 1枚目が開かれたマークの正解のlabelを表示する
      #p-#{$p}:checked
        ~ #check-#{$m}-correct:not(:checked)
        ~ #check-#{$c}-#{$m}-#{$p}-open:checked
        ~ .container
        .game
        .card:not(.c-#{$c})
        .m-#{$m}.correct {
        display: block;
      }

      // 1枚目が開かれたマークでない不正解のlabelを表示する
      #p-#{$p}:checked
        ~ #check-#{$m}-correct:not(:checked)
        ~ #check-#{$c}-#{$m}-#{$p}-open:checked
        ~ .container
        .game
        .card:not(.c-#{$c})
        .p-#{$p}.wrong {
        display: block;
      }
    }
  }
}

// 不正解のカードをめくる
@for $c from 1 through 16 {
  @for $p from 1 through 4 {
    #p-#{$p}:checked
      ~ #check-#{$c}-#{$p}-wrong:checked
      ~ .container
      .game
      .cards
      .c-#{$c} {
      transform: rotateY(180deg) rotateZ(-3deg);
    }
  }
}

$arr: (
  1: (
    1: 7,
    2: 8,
    3: 3,
    4: 3,
    5: 1,
    6: 2,
    7: 4,
    8: 4,
    9: 8,
    10: 7,
    11: 2,
    12: 5,
    13: 5,
    14: 6,
    15: 6,
    16: 1
  ),
  2: (
    1: 5,
    2: 7,
    3: 6,
    4: 5,
    5: 4,
    6: 1,
    7: 7,
    8: 6,
    9: 1,
    10: 4,
    11: 3,
    12: 2,
    13: 2,
    14: 3,
    15: 8,
    16: 8
  ),
  3: (
    1: 3,
    2: 7,
    3: 7,
    4: 4,
    5: 5,
    6: 3,
    7: 2,
    8: 1,
    9: 8,
    10: 2,
    11: 1,
    12: 6,
    13: 4,
    14: 8,
    15: 6,
    16: 5
  ),
  4: (
    1: 8,
    2: 5,
    3: 7,
    4: 7,
    5: 5,
    6: 6,
    7: 2,
    8: 8,
    9: 4,
    10: 1,
    11: 6,
    12: 3,
    13: 3,
    14: 2,
    15: 1,
    16: 4
  ),
  5: (
    1: 8,
    2: 8,
    3: 2,
    4: 2,
    5: 6,
    6: 7,
    7: 7,
    8: 3,
    9: 3,
    10: 6,
    11: 4,
    12: 5,
    13: 1,
    14: 4,
    15: 5,
    16: 1
  ),
  6: (
    1: 7,
    2: 4,
    3: 4,
    4: 6,
    5: 1,
    6: 8,
    7: 5,
    8: 3,
    9: 3,
    10: 2,
    11: 8,
    12: 5,
    13: 2,
    14: 7,
    15: 6,
    16: 1
  ),
  7: (
    1: 7,
    2: 3,
    3: 4,
    4: 6,
    5: 1,
    6: 4,
    7: 2,
    8: 5,
    9: 8,
    10: 7,
    11: 6,
    12: 2,
    13: 5,
    14: 1,
    15: 3,
    16: 8
  ),
  8: (
    1: 6,
    2: 7,
    3: 5,
    4: 2,
    5: 2,
    6: 1,
    7: 1,
    8: 6,
    9: 4,
    10: 8,
    11: 3,
    12: 5,
    13: 8,
    14: 3,
    15: 7,
    16: 4
  )
);

@each $i, $marks in $arr {
  @each $k, $m in $marks {
    #radio-#{($k+1) % 2 + 1}-#{$i}:checked {
      ~ .container .game .c-#{$k} {
        .m-#{$m}.face,
        .m-#{$m}.open {
          display: block;
        }
        :not(.m-#{$m}).correct {
          display: none !important;
        }
      }
      ~ #check-#{$m}-correct:checked ~ .container .game .c-#{$k} {
        transform: rotateY(180deg) rotateZ(-3deg);
      }
    }
  }
}

@keyframes toptobottom {
  0% {
    top: -100%;
  }
  100% {
    top: 0;
  }
}

@keyframes flashing {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@keyframes fill {
  0% {
    height: 0;
  }
  100% {
    height: 100%;
  }
}
</style>
