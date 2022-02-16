<script setup>
// This starter template is using Vue 3 <script setup> SFCs

import { ref, reactive, computed } from "@vue/reactivity";
import { onMounted, watchEffect, watch } from "@vue/runtime-core";
import _ from "lodash";

const inputs = reactive([
  {
    id: 1,
    value: "Delete me!",
    type: "text",
    placeholder: "I am text type",
    error: "Please fill the empty field!",
  },
  {
    id: 2,
    value: "12345",
    type: "number",
    placeholder: "I am number type",
    error: "Please fill the empty field!",
  },
]);

const itemRefs = ref([]);

const autoSave = _.debounce((i) => {
  if (
    !inputs[i].value ||
    (inputs[i].value.length > 4 && inputs[i].value.length < 8)
  )
    return;

  const el = itemRefs.value[i];
  // el.setSelectionRange(0, el.value.length);
  el.select();
  console.log(el.value, "Updated!");
}, 2000);

onMounted(() => {
  console.log(itemRefs.value);
  itemRefs.value[1].focus();
});
</script>

<template>
  <h1 class="text-2xl font-bold mb-6 text-gray-800">
    The Input with Validation
  </h1>
  <div class="flex flex-col gap-4 md:w-1/2">
    <div v-for="(input, i) in inputs" :key="input.id">
      <input
        :ref="(el) => (itemRefs[i] = el)"
        :placeholder="input.placeholder"
        :type="input.type"
        v-model.trim="input.value"
        class="border p-2 outline-none focus:ring rounded"
        :class="[
          !input.value
            ? 'border-red-500 ring-red-200'
            : 'border-blue-500 ring-blue-200',
        ]"
        :title="input.placeholder"
        @input="autoSave(i)"
      />
      <div v-if="false" class="text-red-500 mt-3 text-sm">
        {{ input.error }}
      </div>
    </div>
  </div>
</template>



<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 60px;
  padding: 0 20px;
}
</style>
