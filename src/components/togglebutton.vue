<script setup>
import { ref } from "vue";
import { defineEmits } from "vue";

const emit = defineEmits(["toggleFileType"]);
const fileType = ref(false);

function toggleFileType() {
  fileType.value = !fileType.value;
  emit("toggleFileType", fileType.value);
}
</script>

<template>
  <div class="ml-auto">
    <label class="label cursor-pointer">
      <span class="label-text">Feedback</span>
      <input type="checkbox" @click="toggleFileType" class="toggle mx-9" />
      <span class="label-text">Tarefa</span>
    </label>
  </div>
</template>
