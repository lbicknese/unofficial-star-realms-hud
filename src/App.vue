<template>
  <div id="tsparticles" style="position: fixed; z-index: -1;"></div>
  <div
    class="numpad">
    <button
      type="button"
      @click="select('authority')"
      class="btn attribute authority"
      :class="{ active: attribute === 'authority' }">
      <span>{{ authority }}</span>
      <small>Authority</small>
    </button>
    <button
      type="button"
      @click="select('trade')"
      class="btn attribute trade"
      :class="{ active: attribute === 'trade' }">
        <span>{{ trade }}</span>
        <small>Trade</small>
    </button>
    <button
      type="button"
      @click="select('combat')"
      class="btn attribute combat"
      :class="{ active: attribute === 'combat' }">
      <span>{{ combat }}</span>
      <small>Combat</small>
    </button>
    <input
      type="number"
      class="input value"
      v-model="value">
    <button 
      v-for="num in [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]"
      :key="num"
      type="button"
      class="btn"
      :class="[ 'num-' + num.toString() ]"
      @click="input(num)">
      {{ num }}
    </button>
    <button 
      type="button"
      class="btn add"
      @click="add">
      &#43;
    </button>
    <button 
      type="button"
      class="btn subtract"
      @click="subtract">
      &#45;
    </button>
    <button 
      type="button"
      class="btn set"
      @click="set">
      &rArr;
    </button>
    <button 
      type="button"
      class="btn del"
      @click="del">
      &larr;
    </button>
    <button 
      type="button"
      class="btn clear"
      @click="clear">
      &#10006;
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { tsParticles } from "tsparticles";
import particlesConfig from './particles.config';

type Attribute = 'authority' | 'trade' | 'combat' | undefined;

interface Data {
  value: string;
  attribute: Attribute
  authority: number;
  trade: number;
  combat: number;
}

export default defineComponent({
  name: 'App',
  data(): Data {
    return {
      value: '',
      attribute: undefined,
      authority: 0,
      trade: 0,
      combat: 0
    }
  },
  computed: {
    parsedValue(): number {
      return parseInt(this.value, 10);
    }
  },
  mounted() {
    tsParticles.load("tsparticles", particlesConfig);
  },
  methods: {
    select(value: Attribute) {
      if (this.attribute === value) {
        this.attribute = undefined
      } else {
        this.attribute = value;
      }
    },
    add() {
      if (this.attribute && !isNaN(this.parsedValue)) {
        this[this.attribute] += this.parsedValue;
      }
      this.clear();
    },
    subtract() {
      if (this.attribute && !isNaN(this.parsedValue)) {
        this[this.attribute] -= this.parsedValue;
      }
      this.clear();
    },
    set() {
      if (this.attribute && !isNaN(this.parsedValue)) {
        this[this.attribute] = this.parsedValue;
      }
      this.clear();
    },
    input(value: number) {
      this.value += value.toString();
    },
    del() {
      this.value = this.value.substring(0, this.value.length - 1);
    },
    clear() {
      this.value = '';
    }
  }
});
</script>

<style lang="scss">
$bg-color-default: #123;
$color-default: #fff;
$color-primary: #4151B5;
$color-accent: #06B5C2;
$color-authority: #73B477;
$color-trade: #E7D667;
$color-combat: #C9382D;

body {
  margin: 0;
  background-color: $bg-color-default;
  color: $color-default;
}
* {
  box-sizing: border-box;
}
.btn {
  border: none;
  margin: 0;
  font-size: large;
  background-color: lighten($color: $bg-color-default, $amount: 75);
  overflow: hidden;

  &.attribute {
    font-size: larger;
    font-weight: bold;
    border-width: 5px;
    border-style: solid;
    display: flex;
    flex-flow: wrap;
    align-items: center;
    justify-content: center;
    
    small {
      width: 9ch;
      font-size: x-small;
    }
    
    &.authority {
      border-color: $color-authority;
      background-color: $color-authority;
      &.active {
        border-color: darken($color: $color-authority, $amount: 25);
      }
    }

    &.trade {
      border-color: $color-trade;
      background-color: $color-trade;
      &.active {
        border-color: darken($color: $color-trade, $amount: 25);
      }
    }

    &.combat {
      border-color: $color-combat;
      background-color: $color-combat;
      &.active {
        border-color: darken($color: $color-combat, $amount: 25);
      }
    }
  }

  &:focus {
    outline: unset;
  }
}
.input {
  border: 2px solid $color-primary;
  background-color: lighten($bg-color-default, 10);
  color: $color-default;
  font-size: large;

  &:focus {
    outline: 3px solid $color-accent;
  }
}
.numpad {
  height: calc(100vh - 2em);
  margin: 1em;
  display: grid;
  gap: 0.5em;
  grid-template-rows: minmax(20px, 100px) minmax(20px, 50px) minmax(20px, 50px) minmax(20px, 50px) minmax(20px, 50px) minmax(20px, 50px);
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-template-areas: 
    "authority authority authority authority trade trade trade trade combat combat combat combat"
    "value value value value value value value value value value value value"
    "one one one two two two three three three add add add"
    "four four four five five five six six six subtract subtract subtract"
    "seven seven seven eight eight eight nine nine nine set set set"
    "zero zero zero del del del clear clear clear . . .";
}
.num-1 {
  grid-area: one;
}
.num-2 {
  grid-area: two;
}
.num-3 {
  grid-area: three;
}
.num-4 {
  grid-area: four;
}
.num-5 {
  grid-area: five;
}
.num-6 {
  grid-area: six;
}
.num-7 {
  grid-area: seven;
}
.num-8 {
  grid-area: eight;
}
.num-9 {
  grid-area: nine;
}
.num-0 {
  grid-area: zero;
}
.add {
  grid-area: add;
}
.subtract {
  grid-area: subtract;
}
.set {
  grid-area: set;
}
.del {
  grid-area: del;
}
.clear {
  grid-area: clear;
}
.value {
  grid-area: value;
}
.authority {
  grid-area: authority;
}
.trade {
  grid-area: trade;
}
.combat {
  grid-area: combat;
}
</style>
