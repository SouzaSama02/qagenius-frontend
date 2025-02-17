<script setup>
import { ref } from "vue";

import inputqa from "./components/inputqa.vue";
import outputqa from "./components/outputqa.vue";
import introduce from "./components/introduce.vue";
import togglebutton from "./components/togglebutton.vue";

const outputMessage = ref("");
const loading = ref(false);
const onToggleFileType = ref(false);

function setFileType(value) {
  onToggleFileType.value = value;
}

function setLoading(value) {
  loading.value = value;
}

function setResponseData(value) {
  outputMessage.value = value;
}
</script>

<template>
  <div class="mockup-browser bg-base-300 border">
    <div class="mockup-browser-toolbar">
      <div class="ml-auto">
        <togglebutton @toggleFileType="setFileType" />
      </div>
    </div>
    <div class="bg-white justify-center px-4 py-8">
      <introduce class="mb-12" />
      <div
        class="flex flex-col md:flex-row items-center justify-center space-y-6 md:space-y-0 md:space-x-8"
      >
        <div class="w-full md:w-auto">
          <inputqa
            :fileType="onToggleFileType"
            @sendOutput="setResponseData"
            @loading="setLoading"
          />
        </div>

        <div class="w-full md:w-auto">
          <outputqa :dados="outputMessage" :loading="loading" />
        </div>
      </div>
    </div>
  </div>
</template>
