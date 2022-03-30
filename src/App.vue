<template>
  <div id="app">
    <div class="calculator">
      <div class="header">
        <h1 class="title">calc</h1>
      </div>
      <div class="screen">
        <input
          type="text"
          ref="input"
          class="screen-input"
          v-model="calculate"
          @keypress.prevent="pressed($event.key)"
        />
      </div>
      <div class="calculates" v-if="calculates.length">
        <div class="transactions" v-if="calculatesForPrint.length > 2">
          <span v-for="(transaction, i) in calculatesForPrint" :key="i">{{
            transaction
          }}</span>
          <span>=</span>
          <span>{{ answer }}</span>
        </div>
        <div class="answer">
          {{ answer }}
        </div>
      </div>
      <div class="keypad">
        <div class="buttons">
          <keypad-button :action="7" @pressed="pressed" />
          <keypad-button :action="8" @pressed="pressed" />
          <keypad-button :action="9" @pressed="pressed" />
          <keypad-button
            action="del"
            class="text secondary"
            @pressed="pressed"
          />
          <keypad-button :action="4" @pressed="pressed" />
          <keypad-button :action="5" @pressed="pressed" />
          <keypad-button :action="6" @pressed="pressed" />
          <keypad-button action="+" @pressed="pressed" />
          <keypad-button :action="1" @pressed="pressed" />
          <keypad-button :action="2" @pressed="pressed" />
          <keypad-button :action="3" @pressed="pressed" />
          <keypad-button action="-" @pressed="pressed" />
          <keypad-button action="." @pressed="pressed" />
          <keypad-button :action="0" @pressed="pressed" />
          <keypad-button action="/" @pressed="pressed" />
          <keypad-button action="*" @pressed="pressed" />
          <keypad-button
            action="reset"
            class="half text secondary"
            @pressed="pressed"
          />
          <keypad-button action="=" class="half tertiary" @pressed="pressed" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import KeypadButton from "./components/KeypadButton.vue";

export default {
  name: "App",
  components: {
    KeypadButton,
  },
  data() {
    return {
      calculate: "",
      calculates: [],
      transactions: ["+", "-", "*", "/", "=", "enter"],
      history: [],
      reset: false,
    };
  },
  methods: {
    pressed: function (key) {
      if (isNaN(key)) {
        key = key.toString().toLowerCase();
      }
      if (key == "enter") {
        key = "=";
      }
      if (this.reset) {
        this.history.push(this.calculates);
        this.calculate = "";
        this.calculates = [];
        this.reset = false;
      }
      if (!isNaN(key)) {
        this.calculate += key.toString();
      } else if (key == "." && !this.calculate.includes(".")) {
        this.calculate += key.toString();
      } else if (key == "del") {
        this.calculate = "";
      } else if (key == "reset") {
        this.history.push(this.calculates);
        this.calculate = "";
        this.calculates = [];
      } else if (
        this.transactions.includes(key) &&
        this.calculates.length &&
        this.calculate == ""
      ) {
        this.calculates.pop();
        this.calculates.push(key);
      } else if (
        this.transactions.includes(key) &&
        this.calculate &&
        !isNaN(this.calculate)
      ) {
        let number = 0;
        if (this.calculate.includes(".") || this.calculate.includes(",")) {
          number = parseFloat(this.calculate);
        } else {
          number = parseInt(this.calculate);
        }
        this.calculates.push(number);
        this.calculates.push(key);
        this.calculate = "";
        if (key == "=") {
          this.reset = true;
        }
      }
      this.$refs.input.focus();
    },
  },
  computed: {
    answer: function () {
      if (this.calculates.length) {
        let calculates = this.calculates;
        let answer = calculates[0];
        calculates.forEach(function (val, index) {
          if (!isNaN(calculates[index + 1])) {
            if (val == "+") {
              answer += calculates[index + 1];
            } else if (val == "-") {
              answer -= calculates[index + 1];
            } else if (val == "*") {
              answer *= calculates[index + 1];
            } else if (val == "/") {
              answer /= calculates[index + 1];
            }
          }
        });

        return answer;
      }

      return "";
    },
    calculatesForPrint: function () {
      let arr = [...this.calculates];
      return arr.slice(0, -1);
    },
  },
  watch: {
    calculate: function (val, old) {
      let last = val.slice(-1);
      if ((isNaN(val) && last != ".") || val.length > 15) {
        this.calculate = old;
      }
    },
  },
  created() {
    document.addEventListener("keypress", (event) => {
      console.log(event);
      event.preventDefault();
      if (this.$refs.input !== document.activeElement) {
        this.pressed(event.key);
      }
    });
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Spartan:wght@700&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  height: 100%;
}

body,
input,
select,
button,
textarea {
  font-family: "Spartan", sans-serif;
}

body {
  background-color: #3a4764;
  height: 100%;
  font-size: 32px;
  font-weight: 700;
}

#app {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  color: #fff;
}

.calculator {
  margin: 0 auto;
  padding: 0 15px;
  width: 100%;
  max-width: 600px;
}

.calculator .header {
  margin-bottom: 40px;
}

.calculator .header .title {
  color: #fff;
  font-size: 32px;
}

.screen {
  width: 100%;
}

.screen .screen-input {
  margin-bottom: 30px;
  border: 0;
  border-radius: 10px;
  background-color: #182034;
  padding: 40px;
  width: 100%;
  resize: none;
  text-align: end;
  color: #fff;
  font-size: 48px;
  -webkit-appearance: none;
  appearance: none;
}

.screen .screen-input::-webkit-outer-spin-button,
.screen .screen-input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

.screen .screen-input:focus {
  outline: 0;
}

.calculates {
  margin-bottom: 30px;
  border: 0;
  border-radius: 10px;
  background-color: #182034;
  padding: 20px;
  width: 100%;
  text-align: end;
  line-height: 1;
  color: #fff;
  font-size: 32px;
}

.calculates .transactions {
  margin-bottom: 20px;
  overflow-x: auto;
  line-height: 1.2;
  font-size: 20px;
}

.keypad {
  border-radius: 10px;
  background-color: #232c43;
  padding: 25px 25px 0 25px;
  width: 100%;
}

.keypad .buttons {
  display: flex;
  flex-wrap: wrap;
  margin: 0 -12.5px;
}
</style>

