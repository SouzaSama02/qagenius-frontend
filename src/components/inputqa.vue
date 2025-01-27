<script setup>
import { GoogleGenerativeAI } from "@google/generative-ai";
import { ref } from "vue";
import promptData from "../assets/prompt.json";

const key = import.meta.env.VITE_GOOGLE_GEN_AI_KEY;
const emit = defineEmits(["sendOutput"]);

const inputText = ref("");
const exibirresposta = ref("");

async function prompt() {
  try {
    const genAI = new GoogleGenerativeAI(key);
    const model = genAI.getGenerativeModel({ model: "gemini-1.5-flash" });

    const prompt = `
      ${promptData.instruction}
      ${Object.entries(promptData.details)
        .map(([key, value]) => `${key}: ${value}`)
        .join("\n")}
      Informação adicional: ${inputText.value}
      Formato esperado: ${promptData.format}
    `;
    const result = await model.generateContent(prompt);

    exibirresposta.value = result.response.text() || "Sem Resposta";
    emit("sendOutput", { response: exibirresposta.value, success: true });
  } catch (error) {
    console.log("Erro ao enviar resposta:", error);
    exibirresposta.value = "Erro ao processar a resposta. Tente novamente.";
    emit("sendOutput", exibirresposta.value);
  }
}
</script>

<template>
  <div class="w-auto flex flex-col items-center">
    <textarea
      id="inputText"
      class="textarea textarea-primary"
      placeholder="Input"
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
