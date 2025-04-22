<template>
  <div @click="edit" v-if="!isEditing" class="date-picker-field">
    {{ value }}
  </div>
  <input
      v-else
      type="date"
      v-model="inputValue"
      @blur="save"
      @keyup.enter="save"
      class="edit-input"
  />
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'DatePickerField',
  props: {
    value: {
      type: String,
      required: true,
    },
  },
  setup(props, { emit }) {
    const isEditing = ref(false);
    const inputValue = ref(props.value);

    const edit = () => {
      isEditing.value = true;
    };

    const save = () => {
      if (inputValue.value) {
        emit('update', inputValue.value);
      }
      isEditing.value = false;
    };

    return {
      isEditing,
      inputValue,
      edit,
      save,
    };
  },
});
</script>

<style scoped>
.date-picker-field {
  font-size: 1rem;
  cursor: pointer;
}

.edit-input {
  font-size: 1rem;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
