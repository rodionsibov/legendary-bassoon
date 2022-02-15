<script setup>
// This starter template is using Vue 3 <script setup> SFCs

import { ref } from "@vue/reactivity";
import { onMounted, watchEffect } from "@vue/runtime-core";
import _ from "lodash";

const input = ref(null);
const value = ref("Delete me!");

const autoSave = _.debounce(() => {
  input.value.setSelectionRange(0, input.value.value.length);
  console.log("Saved");
}, 2000);

onMounted(() => {
  input.value.focus();
});
</script>

<template>
  <h1 class="text-2xl font-bold mb-8 text-gray-800">
    The Input with Validation
  </h1>

  <input
    placeholder="Type something here..."
    type="text"
    ref="input"
    v-model="value"
    @input="autoSave"
    class="border p-2 outline-none focus:ring rounded"
    :class="[
      !value ? 'border-red-500 ring-red-200' : 'border-blue-500 ring-blue-200',
    ]"
  />
  <div v-if="!value" class="text-red-500 mt-3 text-sm">
    Please fill the empty field!
  </div>
</template>



<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}
</style>
