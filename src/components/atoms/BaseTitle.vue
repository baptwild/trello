<template>
  <component
      :is="tag"
      :class="['base-title', variant]"
      @click="$emit('click')"
  >
    <slot />
  </component>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

export default defineComponent({
  name: 'BaseTitle',
  props: {
    tag: {
      type: String as PropType<keyof HTMLElementTagNameMap>,
      default: 'h2',
    },
    variant: {
      type: String as PropType<'h1' | 'h2' | 'h5' | ''>,
      default: '',
    },
  },
  emits: ['click']
});
</script>

<style scoped>
.base-title {
  font-weight: bold;
  margin: 0;
  padding: 0;
}

.base-title.h1 {
  font-size: 2rem;
}

.base-title.h2 {
  font-size: 1.5rem;
}

.base-title.h5 {
  font-size: 1.1rem;
  cursor: pointer;
}
</style>
