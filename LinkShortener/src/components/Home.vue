<template>
  <div
    class="max-h-[500px] max-w-[500px] rounded-lg border-8 border-white px-2 py-3"
  >
    <h1 class="text-2xl">Link Shortener</h1>
    <p class="mt-1 text-lg">
      Put any URL link you would like to shorten inside the box below.
    </p>
    <input
      type="text"
      v-model="inputUrl"
      placeholder="Put in your link here..."
      class="mt-5 w-full rounded-[20px] border-2 border-[#B4CDED] py-3 pl-3 pr-2 shadow-sm outline-none transition-colors focus:border-[#7ba0d0] md:pl-5"
    />
    <div
      class="mt-4 flex flex-col justify-between gap-x-2 gap-y-2 md:flex-row md:gap-y-0"
    >
      <ActionButton @click="shortenURL">Generate Short Link</ActionButton>
      <ActionButton @click="clearInput"> Clear the input </ActionButton>
    </div>
    <p v-show="isReturnedUrl" class="mt-2 text-lg">
      <span>Shortened Link: </span
      ><a target="_blank" :href="returnedUrl" class="underline">{{
        returnedUrl
      }}</a>
    </p>
    <ActionButton v-show="isReturnedUrl" @click="copy" class="mt-1"
      >Copy shortened link</ActionButton
    >
  </div>
</template>

<script setup>
import ActionButton from "./ActionButton.vue";

import { ref } from "vue";

const inputUrl = ref("");
const returnedUrl = ref("");
const isReturnedUrl = ref(false);

const shortenURL = async () => {
  const requestURL = "https://api-ssl.bitly.com/v4/shorten";
  const longURL = inputUrl.value;
  const options = {
    method: "POST",
    headers: {
      Authorization: `Bearer ${import.meta.env.VITE_KEY}`,
      "Access-Control-Allow-Headers": "*",
      "Content-Type": "application/json",
    },
    body: JSON.stringify({ long_url: `${longURL}`, domain: "bit.ly" }),
  };

  const response = await fetch(requestURL, options);
  if (response.ok) {
    const data = await response.json();
    returnedUrl.value = data.link;
    isReturnedUrl.value = true;
  } else {
    console.log(error);
  }
};

const copy = () => {
  navigator.clipboard.writeText(returnedUrl.value);
};

const clearInput = () => {
  inputUrl.value = "";
  returnedUrl.value = "";
  isReturnedUrl.value = false;
};
</script>

<style scoped></style>
