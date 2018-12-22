<template>
  <form>
    <!-- お手付きの回数 -->
    <input 
      v-for="p in phaseCount" 
      :key="p" 
      :checked="1==p"
      :id="`p-${p}`"
      :class="`p-${p}`"
      name="p"
      type="radio">

    <!-- カードの正解分に対応するcheckbox -->
    <template
      v-for="g in 2">
      <template
        v-for="c in cardCount">
        <input 
          v-for="p in phaseCount" 
          :class="['correct', `g-${g}`, `p-${p}`, `c-${c}`]" 
          :key="[c, g, p]"
          :id="`c-${c}-${g}-${p}`" 
          type="checkbox">
      </template>
    </template>

    <!-- カードに対応するcheckbox -->
    <template
      v-for="g in 2">
      <template
        v-for="c in cardCount">
        <input 
          v-for="p in phaseCount" 
          :class="[`g-${g}`, `p-${p}`, `c-${c}`]" 
          :key="[c, g, p]"
          :id="`c-${c}-${g}-${p}`" 
          type="checkbox">
      </template>
    </template>



    <div>
      <h1 class="title">
        css-memory-game
      </h1>
      <div>
        <!-- カードの種類数 * お手付きの上限 のカードを配置 -->
        <div class="container">
          <template
            v-for="g in 2">
            <template
              v-for="c in cardCount">
              <div
                :key="[c,g]"
                :class="['card', `g-${g}`, `c-${c}`]">
                <label 
                  v-for="p in phaseCount"
                  :key="p"
                  :class="[`r-1`, `l-${p}`]"
                  :for="`c-${c}-${g}-${p}`" />
                <div class="front">
                  <span>{{ c }}</span>
                </div>
                <div class="back"/>
              </div>

              <!-- カードの種類数出したら改行 -->
            </template>
            <br :key="g">
          </template>

          <!-- お手付きのダイアログ -->
          <div class="fail-dialog">
            <label 
              for="p-2" 
              class="p-2-l">お手付き 残り2</label> 
            <label 
              for="p-3" 
              class="p-3-l">お手付き 残り1</label>
          </div>
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
      phaseCount: 3,
      cardCount: 8
    }
  },
  computed: {
    name: function(pre, count) {
      return pre + count
    }
  }
}
</script>

<style lang="scss">
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

@for $j from 1 through 3 {
  /* カードに対応するのlabelの非表示 */
  .r-1-#{$j}:checked ~ .container .r-1:not(.l-#{$j}) {
    display: none;
  }

  @for $g from 1 through 2 {
    @for $i from 1 through 8 {
      /* .r-1-1:checked ~ #c-1-1-1:checked ~ .container .card.c-1-a */
      .r-1-#{$j}:checked
        ~ #c-#{$i}-#{$g}-#{$j}:checked
        ~ .container
        .card.c-#{$i}.g-#{$g} {
        transform: rotateY(180deg) rotateZ(-3deg);
      }

      /* お手付きのダイアログの表示 */
      .r-1-#{$j}:checked
        ~ .c-#{$i}.g-#{$g}.p-#{$j}:checked
        ~ .c-#{$i}:not(.g-#{$g}).p-#{$j}:not(:checked)
        ~ :not(.c-#{$i}).p-#{$j}:checked
        ~ .container
        .fail-dialog {
        display: block;
      }

      .r-1-#{$j}:checked
        ~ .c-#{$i}.g-#{$g}.p-#{$j}:checked
        ~ :not(.c-#{$i}).p-#{$j}:checked
        ~ .c-#{$i}:not(.g-#{$g}).p-#{$j}:not(:checked)
        ~ .container
        .fail-dialog {
        display: block;
      }

      .r-1-#{$j}:checked
        ~ .g-#{$g}.p-#{$j}:checked
        ~ :not(.g-#{$g}.p-#{$j}):checked
        ~ .container
        .r-1.l-#{$j
        + 1} {
        display: inline-block;
      }
    }
  }
}

@for $c from 1 through 16 {
  @for $p from 1 through 3 {
    .g-#{$c%8}:nth-child(3n - #{$p}):checked
      ~ .g-#{$c%8}:nth-child(3n - #{$p}):checked
      ~ .container
      .g-#{$c%8}-a {
      visibility: hidden;
    }
  }
}
</style>
