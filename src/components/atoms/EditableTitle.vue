<template>
  <div class="editable-title">
    <input
        v-if="isEditing"
        v-model="localValue"
        @blur="save"
        @keyup.enter="save"
        ref="input"
        class="edit-input"
    />
    <BaseTitle
        v-else
        :level="level"
        :text="localValue"
        @click="edit"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch, nextTick } from 'vue';
import BaseTitle from './BaseTitle.vue';

export default defineComponent({
  name: 'EditableTitle',
  components: { BaseTitle },
  props: {
    modelValue: {
      type: String,
      required: true
    },
    level: {
      type: String,
      default: 'h2'
    }
  },
  emits: ['update:modelValue'],
  setup(props, { emit }) {
    const isEditing = ref(false);
    const localValue = ref(props.modelValue);
    const input = ref<HTMLInputElement | null>(null);

    watch(() => props.modelValue, (newVal) => {
      localValue.value = newVal;
    });

    const edit = () => {
      isEditing.value = true;
      nextTick(() => {
        input.value?.focus();
      });
    };

    const save = () => {
      isEditing.value = false;
      emit('update:modelValue', localValue.value.trim() || props.modelValue);
    };

    return {
      isEditing,
      localValue,
      input,
      edit,
      save
    };
  }
});
</script>

<style scoped>
.edit-input {
  font-size: 1.2rem;
  font-weight: bold;
  padding: 4px;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 100%;
}
</style>
