<template>
  <div class="calculator">
    <Display :display="Number(display)" />
    <div class="grid">
      <button v-for="btn in buttons" :label="btn" @click="press(btn)">
        {{ btn }}
      </button>
    </div>
  </div>
</template>
<script setup>
import Display from "./Display.vue";
import { ref, computed } from "vue";

//We assign the 3 main things we need
const current = ref(0);
const storedValue = ref(null);
const operator = ref(null);

const buttons = [
  "7",
  "8",
  "9",
  "/",
  "4",
  "5",
  "6",
  "*",
  "1",
  "2",
  "3",
  "-",
  "0",
  ".",
  "=",
  "+",
  "C",
];

const display = computed(() => Number(current.value));

function press(btn) {
  if (!isNaN(btn) || btn === ".") {
    appendNum(btn);
  } else if (btn === "=") {
    calculate();
  } else if (btn === "C") {
    clearAll();
  } else {
    appendOperator(btn);
  }
}

function appendNum(num) {
  if (num === "." && current.value.includes(".")) return;
  if (current.value === "0" && num !== ".") {
    current.value = num; // replace leading 0
  } else {
    current.value += num;
  }
}

function clearAll() {
  current.value = 0;
  storedValue.value = null;
  operator.value = null;
}

function appendOperator(op) {
  operator.value = op;
  storedValue.value = current.value;
  current.value = 0;
}

function calculate() {
  if (storedValue.value !== null && operator.value !== null) {
    const a = Number(storedValue.value);
    const b = Number(current.value);

    switch (operator.value) {
      case "+":
        current.value = String(a + b);
        break;
      case "-":
        current.value = String(a - b);
        break;
      case "/":
        current.value = String(a / b);
        break;
      case "*":
        current.value = String(a * b);
        break;
    }
    storedValue.value = null;
    operator.value = null;
  }
}
</script>
<style>
.calculator {
  max-width: 200px;
  margin: auto;
}
.display {
  background: #222;
  color: #fff;
  font-size: 1.5rem;
  text-align: right;
  padding: 0.5rem;
  margin-bottom: 0.5rem;
}
.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 0.5rem;
}
</style>
