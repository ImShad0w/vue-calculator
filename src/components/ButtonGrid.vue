<template>
  <div class="grid">
    <button
      v-for="button in buttons"
      :class="button"
      @click="emit('clickedButton', button)"
    >
      {{ button }}
    </button>
  </div>
</template>
<script setup>
const props = defineProps({
  buttons: Array,
});
const emit = defineEmits(["clickedButton"]);
</script>
