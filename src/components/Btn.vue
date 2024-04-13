<template>
  <div class="btn" @click.native="onClick" :class="{ flat, disabled, fab }">
    <Icon v-if="icon">{{ icon }}</Icon>
    <slot>{{ label }}</slot>
    <div class="ripple" :class="{ active: ripple.state }"></div>
  </div>
</template>

<script setup>
import Icon from "@/components/Icon.vue";

const props = defineProps({
  label: {
    type: String,
    default: undefined,
  },
  icon: {
    type: String,
    default: undefined,
  },
  flat: {
    type: Boolean,
    default: false,
  },
  disabled: {
    type: Boolean,
    default: false,
  },
  fab: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits(["click"]);

const ripple = ref({
  state: false,
  x: 0,
  y: 0,
});

let rippleTimeout = null;

function onClick(e) {
  ripple.value.state = true;
  ripple.value.x = [e.clientX - e.target.offsetLeft, "px"].join("");
  ripple.value.y = [e.clientY - e.target.offsetTop, "px"].join("");

  if (rippleTimeout) {
    clearTimeout(rippleTimeout);
  }
  setTimeout(() => (ripple.value.state = false), 300);

  emit("click");
}
</script>

<style scoped lang="scss"></style>