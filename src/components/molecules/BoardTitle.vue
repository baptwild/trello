<template>
  <BaseTitle tag="h1" variant="h1">
    {{ title }}
  </BaseTitle>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import BaseTitle from '../atoms/BaseTitle.vue';

export default defineComponent({
  name: 'BoardTitle',
  components: { BaseTitle },
  props: {
    title: {
      type: String,
      required: true
    }
  }
});
</script>
