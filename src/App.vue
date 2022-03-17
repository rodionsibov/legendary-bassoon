<script setup>
// This starter template is using Vue 3 <script setup> SFCs

import { ref, reactive, computed } from "@vue/reactivity";
import { onMounted, watchEffect, watch } from "@vue/runtime-core";
import { log } from "cypress/lib/logger";
import _ from "lodash";

const inputs = reactive([
  {
    id: 1,
    value: "Delete me!",
    type: "text",
    placeholder: "I am text type",
    error: "",
  },
  {
    id: 2,
    value: "12345",
    type: "number",
    placeholder: "I am number type",
    error: "",
  },
  {
    id: 3,
    value: "Delete me!",
    type: "text",
    placeholder: "I am text type",
    error: "",
  },
]);

const messages = ref([]);

const itemRefs = ref([]);
let tempInputValue = [];

const saveInput = (inputValue) => {
  tempInputValue = [];
  tempInputValue.push(inputValue);
};

const checkForm = _.debounce((inputValue, i) => {
  if (inputValue && inputValue !== tempInputValue.join("")) {
    // inputs[i].message = `Input ${i + 1} Success updated`;
    inputs[i].error = "";
    messages.value.unshift(inputValue);

    itemRefs.value[i].blur();
    return true;
  }

  if (!inputs[i].value && inputs[i].type === "text") {
    inputs[i].error = "Name required";
  }

  if (!inputs[i].value && inputs[i].type === "number") {
    inputs[i].error = "Age required";
  }

  // const el = itemRefs.value[i];
  // el.setSelectionRange(0, el.value.length);
  // el.select();
}, 1000);


// directives
const vFocus = {
  mounted: (el) => {
    el.focus();
  },
};

const closeMessage = (message) => {
  messages.value = messages.value.filter((el) => el !== message);
};
</script>

<template>
  <h1 class="text-4xl mb-6 text-gray-800 md:pr-72 text-center">
    The Input
    <span class="font-extrabold">with Validation</span>
  </h1>
  <div class="flex flex-col gap-4 md:w-1/2">
    <div v-for="(input, i) in inputs" :key="input.id" class="mx-auto">
      <input
        v-focus
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
        @input="checkForm(input.value, i)"
        @focus="saveInput(input.value)"
      />
      <div v-if="input.error" class="text-red-500 mt-3 text-sm">
        {{ input.error }}
      </div>
    </div>
    <div
      class="
        flex flex-col
        gap-2
        absolute
        right-0
        bottom-2
        md:top-2 md:w-1/3
        w-11/12
      "
    >
      <div
        v-for="message in messages"
        :key="message"
        @click="closeMessage(message)"
        class="
          text-xs
          bg-green-200
          p-3
          border-l-4 border-green-800
          cursor-pointer
          hover:bg-green-300
          break-all
        "
      >
        <strong>{{ message }}</strong> Success updated
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
