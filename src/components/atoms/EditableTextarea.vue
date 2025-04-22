<template>
  <div @click="edit" v-if="!isEditing" class="editable-text">
    {{ value }}
  </div>
  <textarea
      v-else
      v-model="inputValue"
      @blur="save"
      @keyup.enter="save"
      class="edit-textarea"
      rows="4"
      autofocus
  />
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'EditableTextarea',
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
      if (inputValue.value.trim() !== '') {
        emit('update', inputValue.value);
      } else {
        inputValue.value = props.value; // Revert to original value if empty
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
.editable-text {
  font-size: 1rem;
  cursor: pointer;
}

.edit-textarea {
  width: 100%;
  font-size: 1rem;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  resize: vertical;
}
</style>
