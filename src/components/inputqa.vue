<script setup>
import { ref } from "vue";
import { defineProps, defineEmits } from "vue";
import { computed } from "vue";
import axios from "axios";

const URL_API = import.meta.env.VITE_URL_API;
const emit = defineEmits(["sendOutput", "loading"]);
const props = defineProps({
  fileType: {
    type: Boolean,
    requerid: false,
    default: false,
  },
});

const inputText = ref("");
const exibirresposta = ref("");
const loading = ref(false);
const typeFile = computed(() => (props.fileType ? "task" : "prompt"));

async function prompt() {
  loading.value = true;
  emit("loading", loading.value);
  try {
    const response = await axios.post(`${URL_API}/prompt`, {
      inputText: inputText.value,
      typeFile: typeFile.value,
    });

    exibirresposta.value = response.data.response || "Sem Resposta";
    emit("sendOutput", { response: exibirresposta.value, success: true });
  } catch (error) {
    console.log("Erro ao enviar resposta:", error);
    exibirresposta.value = "Erro ao processar a resposta. Tente novamente.";
    emit("sendOutput", exibirresposta.value);
  } finally {
    loading.value = false;
    emit("loading", loading.value);
  }
}
</script>

<template>
  <div class="w-auto flex flex-col items-center">
    <textarea
      id="inputText"
      class="textarea textarea-primary"
      placeholder="Ex: O botão de login não está funcionando corretamente, e a API retorna erro 400."
      v-model="inputText"
      rows="13"
      cols="80"
    ></textarea>
    <button
      class="btn btn-primary mt-4 py-2 px-16 self-center bg-indigo-600 text-white rounded-md hover:bg-indigo-500 transition"
      @click="prompt"
      :disabled="inputText.trim() === ''"
    >
      Enviar
    </button>
  </div>
</template>
