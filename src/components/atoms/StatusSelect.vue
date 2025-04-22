<template>
  <div @click="edit" v-if="!isEditing" class="status-select-field">
    {{ value }}
  </div>
  <select
      v-else
      v-model="inputValue"
      @blur="save"
      @change="save"
      class="edit-input"
  >
    <option value="To Do">To Do</option>
    <option value="Doing">Doing</option>
    <option value="To Review">To Review</option>
    <option value="Done">Done</option>
  </select>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'StatusSelect',
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
      emit('update', inputValue.value);
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
.status-select-field {
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
