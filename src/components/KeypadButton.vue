<template>
  <div class="keypad-item">
    <button type="button" @click="pressed(action)" class="keypad-button">
      {{ action }}
    </button>
  </div>
</template>

<script>
export default {
  props: ["action"],
  methods: {
    pressed: function (key) {
      this.$emit("pressed", key);
    },
  },
};
</script>

<style>
.keypad-item {
  flex: 0 0 auto;
  margin-bottom: 25px;
  padding: 0 12.5px;
  width: 25%;
}

.keypad-button {
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 500ms;
  border: 0;
  border-bottom: 4px solid #b4a597;
  border-radius: 10px;
  background-color: #eae3dc;
  cursor: pointer;
  padding-top: 20px;
  padding-bottom: 10px;
  width: 100%;
  height: 64px;
  text-transform: uppercase;
  line-height: 1;
  color: #444b5a;
  font-size: 30px;
}

.keypad-button:hover {
  opacity: 0.7;
}

.keypad-item.half {
  width: 50%;
}

.keypad-item.text .keypad-button {
  font-size: 20px;
}

.keypad-item.secondary .keypad-button {
  border-bottom-color: #404e72;
  background-color: #637097;
  color: #fff;
}

.keypad-item.tertiary .keypad-button {
  border-bottom-color: #93261a;
  background-color: #d03f2f;
  color: #fff;
}
</style>
