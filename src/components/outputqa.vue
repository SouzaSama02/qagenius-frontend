<script setup>
import { defineProps } from "vue";

function copyTextArea() {
  if (document.querySelector("#textArea") != null) {
    const textArea = document.querySelector("#textArea");
    navigator.clipboard.writeText(textArea.value);
  }
}

defineProps({
  dados: {
    type: Object,
    required: true,
    default: "",
  },
  loading: {
    type: Boolean,
    required: false,
    default: false,
  },
});
</script>

<template>
  <div class="w-auto flex flex-col items-center">
    <span
      v-if="(loading === true) & (dados === '')"
      class="ml-8 loading loading-bars loading-lg"
    ></span>
    <textarea
      id="textArea"
      v-if="dados"
      class="textarea textarea-primary w-full h-full"
      rows="13"
      cols="80"
      v-model="dados.response"
    >
    </textarea>
    <button
      class="btn btn-primary bg-indigo-600 mt-4 py-2 px-16 self-center text-white rounded-md hover:bg-indigo-500 transition"
      @click="copyTextArea"
      v-if="dados"
    >
      <img src="../assets/paper-svgrepo-com.svg" alt="icon-paper" />
      Copy
    </button>
    <p
      v-else-if="(dados === '') & (loading === false)"
      class="text-gray-500 italic text-center px-4"
    >
      Nenhuma resposta disponível.
    </p>
  </div>
</template>
