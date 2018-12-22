<template>
  <form>
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
      <div class="top">
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
      <div class="game">
        <div class="help">
          <p>
            カードは全8種類<br>
            お手つき3回でGameOverです。
          </p>
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
        Game

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
                  :class="[`m-${m}`]">{{ mark[m-1] }}</span>
              </div>
              <div class="back"/>
            </div>
          </template>
        </div>

        <div class="failed">
          <div class="f-1">
            <label for="p-2"/>
            failed1
          </div>
          <div class="f-2">
            <label for="p-3"/>
            failed2
          </div>
          <div class="f-3">
            failed3
            <input
              type="reset"
              value="">
          </div>
        </div>
        <div class="result">
          Complete!
          <input
            type="reset"
            value="">
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
      phaseCount: 3,
      markCount: 8,
      mark: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
      // cardCount: 16
    }
  },
  computed: {
    name: function(pre, count) {
      return pre + count
    },
    cardCount: function() {
      return this.markCount * 2
    }
  }
}
</script>

<style lang="scss">
// .cards {
// }
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

    span {
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

.fail-dialog {
  display: none;
}

input[type='checkbox']:nth-child(3n) {
  margin-right: 10px;
}

// カードをめくる
@for $c from 1 through 16 {
  @for $m from 1 through 8 {
    @for $p from 1 through 3 {
      #p-#{$p}:checked
        ~ #check-#{$c}-#{$m}-#{$p}-open:checked
        ~ .container
        .game
        .cards
        .c-#{$c} {
        transform: rotateY(180deg) rotateZ(-3deg);
      }
    }
  }
}

// @for $c from 1 through 16 {
//   @for $m from 1 through 8 {
//     @for $p from 1 through 3 {
//       #check-#{$m}-correct:checked
//         ~ .container
//         .game
//         .cards
//         .c-#{$c} {
//         transform: rotateY(180deg) rotateZ(-3deg);
//       }
//     }
//   }
// }

// @for $j from 1 through 3 {
//   /* カードに対応するのlabelの非表示 */
//   .radio-1-#{$j}:checked ~ .container .radio-1:not(.l-#{$j}) {
//     display: none;
//   }

//   @for $g from 1 through 2 {
//     @for $i from 1 through 8 {
//       /* .radio-1-1:checked ~ #check-1-1-1:checked ~ .container .card.check-1-a */
//       .radio-1-#{$j}:checked
//         ~ #check-#{$i}-#{$g}-#{$j}:checked
//         ~ .container
//         .card.check-#{$i}.g-#{$g} {
//         transform: rotateY(180deg) rotateZ(-3deg);
//       }

//       /* お手付きのダイアログの表示 */
//       .radio-1-#{$j}:checked
//         ~ .check-#{$i}.g-#{$g}.p-#{$j}:checked
//         ~ .check-#{$i}:not(.g-#{$g}).p-#{$j}:not(:checked)
//         ~ :not(.check-#{$i}).p-#{$j}:checked
//         ~ .container
//         .fail-dialog {
//         display: block;
//       }

//       .radio-1-#{$j}:checked
//         ~ .check-Q#{$i}.g-#{$g}.p-#{$j}:checked
//         ~ :not(.check-#{$i}).p-#{$j}:checked
//         ~ .check-#{$i}:not(.g-#{$g}).p-#{$j}:not(:checked)
//         ~ .container
//         .fail-dialog {
//         display: block;
//       }

//       .radio-1-#{$j}:checked
//         ~ .g-#{$g}.p-#{$j}:checked
//         ~ :not(.g-#{$g}.p-#{$j}):checked
//         ~ .container
//         .radio-1.l-#{$j
//         + 1} {
//         display: inline-block;
//       }
//     }
//   }
// }

// @for $c from 1 through 16 {
//   @for $p from 1 through 3 {
//     .g-#{$c%8}:nth-child(3n - #{$p}):checked
//       ~ .g-#{$c%8}:nth-child(3n - #{$p}):checked
//       ~ .container
//       .g-#{$c%8}-a {
//       visibility: hidden;
//     }
//   }
// }

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
.failed div,
.result {
  display: none;
}

.failed {
  label {
    display: block;
  }
  div,
  label {
    height: 100%;
  }
}

.label-wrapper {
  position: absolute;
  height: 200%;
  width: 100%;
  top: 0;
  left: 0;
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
  z-index: 10;
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

.game .card label,
.game .card .front span {
  display: none;
}

/* Todo ランダムにならない */
@for $r1 from 1 through 2 {
  @for $r2 from 1 through 8 {
    @for $m from 1 through 8 {
      #radio-#{$r1}-#{$r2}:checked ~ .container .game .c-#{$m} span.m-#{$m},
      #radio-#{$r1}-#{$r2}:checked
        ~ .container
        .game
        .c-#{$m}
        label.m-#{$m}.open {
        display: block;
      }
      #radio-#{$r1}-#{$r2}:checked
        ~ .container
        .game
        .c-#{$m}
        :not(.m-#{$m}).correct {
        display: none !important;
      }
      #radio-#{$r1}-#{$r2}:checked ~ .container .game .c-#{$m + 8} span.m-#{$m},
      #radio-#{$r1}-#{$r2}:checked
        ~ .container
        .game
        .c-#{$m
        + 8}
        label.m-#{$m}.open {
        display: block;
      }
      #radio-#{$r1}-#{$r2}:checked
        ~ .container
        .game
        .c-#{$m
        + 8}
        :not(.m-#{$m}).correct {
        display: none !important;
      }

      #radio-#{$r1}-#{$r2}:checked
        ~ #check-#{$m}-correct:checked
        ~ .container
        .game
        .c-#{$m},
      #radio-#{$r1}-#{$r2}:checked
        ~ #check-#{$m}-correct:checked
        ~ .container
        .game
        .c-#{$m
        + 8} {
        transform: rotateY(180deg) rotateZ(-3deg);
      }
    }
  }
}

// @for $p from 1 through 3 {
//   @for $m from 1 through 8 {
//     #p-#{$p}
//       ~ #check-#{$m}-correct:not(:checked)
//       ~ .m-#{$m}.p-#{$p}:checked
//       ~ .container
//       .game
//       .m-#{$m}.correct {
//       display: block;
//     }
//     #p-#{$p}
//       ~ #check-#{$m}-correct:not(:checked)
//       ~ .m-#{$m}.p-#{$p}:checked
//       ~ .container
//       .game
//       :not(.m-#{$m}).p-#{$p}.wrong {
//       display: block;
//     }
//     #p-#{$p}
//       ~ #check-#{$m}-correct:not(:checked)
//       ~ .m-#{$m}.p-#{$p}:checked
//       ~ .container
//       .game
//       .m-#{$m}.correct
//       ~ .wrong {
//       display: none;
//     }
//   }
// }

@for $c from 1 through 16 {
  @for $m from 1 through 8 {
    @for $p from 1 through 3 {
      #p-#{$p}:checked
        ~ #check-#{$m}-correct:not(:checked)
        ~ #check-#{$c}-#{$m}-#{$p}-open:checked
        ~ .container
        .game
        .m-#{$m}.correct {
        display: block;
      }
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
// @for $p from 1 through 3 {
//   @for $c from 1 through 16 {
//     #p-#{$p}
//       ~ .c-1:checked
//       ~ .container
//       .game
//       .c-#{$c}
//       label.p-#{$p}[for$='wrong'] {
//       display: none !important;
//     }
//   }
// }

#radio-1-1:checked ~ .container .top,
#radio-1-2:checked ~ .container .top,
#radio-1-3:checked ~ .container .top,
#radio-1-4:checked ~ .container .top {
  display: none;
}
#radio-1-1:checked ~ .container .game,
#radio-1-2:checked ~ .container .game,
#radio-1-3:checked ~ .container .game,
#radio-1-4:checked ~ .container .game {
  display: block;
}
#radio-2-1:checked ~ .container .game .help,
#radio-2-2:checked ~ .container .game .help,
#radio-2-3:checked ~ .container .game .help,
#radio-2-4:checked ~ .container .game .help {
  display: none;
}

#p-1:checked ~ .container .card label.p-2,
#p-1:checked ~ .container .card label.p-3,
#p-2:checked ~ .container .card label.p-1,
#p-2:checked ~ .container .card label.p-3,
#p-3:checked ~ .container .card label.p-1,
#p-3:checked ~ .container .card label.p-2 {
  display: none !important;
}

#p-1:checked ~ .container .game .failed .f-1,
#p-2:checked ~ .container .game .failed .f-2,
#p-3:checked ~ .container .game .failed .f-3 {
  display: none;
}

#p-1:checked ~ [id$='wrong'].p-1:checked ~ .container .game .failed,
#p-1:checked ~ [id$='wrong'].p-1:checked ~ .container .game .failed .f-1,
#p-2:checked ~ [id$='wrong'].p-2:checked ~ .container .game .failed,
#p-2:checked ~ [id$='wrong'].p-2:checked ~ .container .game .failed .f-2,
#p-3:checked ~ [id$='wrong'].p-3:checked ~ .container .game .failed,
#p-3:checked ~ [id$='wrong'].p-3:checked ~ .container .game .failed .f-3 {
  display: block;
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
