<template>
  <div>
    <label class="form-control w-full max-w-xs">
      <div class="label">
        <span class="label-text">Jawaban yang benar</span>
        <span class="label-text-alt">Jumlah soal {{ characterCount1 }}</span>
      </div>
      <input
        v-model="inputText1"
        placeholder="Input kunci Jawaban..."
        class="input input-bordered w-full max-w-xs"
      />
      <div class="label">
        <span class="label-text">Hasil jawaban siswa</span>
      </div>
      <input
        v-model="inputText2"
        placeholder="Input Hasil siswa..."
        class="input input-bordered w-full max-w-xs"
        v-bind:maxlength="characterCount1"
      />
      <div class="label">
        <span class="label-text-alt"
          >Jumlah jawaban: {{ characterCount2 }}</span
        >
      </div>
    </label>

    <div>
      <!-- Second input field with v-model to bind the value to the reactive variable -->

      <!-- <p>Processed Text 1: {{ processedText1 }}</p>
      <p>Processed Text 2: {{ processedText2 }}</p> -->
      <!-- 
      <p>Jumlah soal: {{ characterCount1 }}</p>
      <p>Jumlah jawaban: {{ characterCount2 }}</p> -->

      <!-- Display the number of characters that are the same -->
      <p>
        Jumlah Yang Benar:
        <label class="text-green-500 font-bold text-lg">{{
          sameCharacterCount
        }}</label>
      </p>

      <!-- Display the positions of differing characters -->
      <p v-if="differingPositions.length > 0">
        Jawaban yang salah: No.
        <label class="text-red-500 font-bold text-lg">{{
          differingPositions.join(", ")
        }}</label>
      </p>
      <p v-else>Tidak ada yang salah</p>
    </div>
  </div>
</template>

<script setup>
import { ref, watchEffect, computed } from "vue";

// Reactive variables to store the input texts
const inputText1 = ref("");
const inputText2 = ref("");

// Reactive variables to store the processed texts
const processedText1 = ref("");
const processedText2 = ref("");

// Watch for changes in the inputText1 variable
watchEffect(() => {
  // Access the input text through the reactive variable
  const textToProcess = inputText1.value;

  // Perform any processing logic here
  // For example, you can update the processedText1 variable
  processedText1.value = `Processed: ${textToProcess}`;
});

// Watch for changes in the inputText2 variable
watchEffect(() => {
  // Access the input text through the reactive variable
  const textToProcess = inputText2.value;

  // Perform any processing logic here
  // For example, you can update the processedText2 variable
  processedText2.value = `Processed: ${textToProcess}`;
});

// Computed properties to calculate the character count for each text
const characterCount1 = computed(() => {
  return inputText1.value.length;
});

const characterCount2 = computed(() => {
  return inputText2.value.length;
});

// Computed property to calculate the number of characters that are the same
const sameCharacterCount = computed(() => {
  const len = Math.min(inputText1.value.length, inputText2.value.length);
  let count = 0;

  for (let i = 0; i < len; i++) {
    if (inputText1.value[i] === inputText2.value[i]) {
      count++;
    }
  }

  return count;
});

// Computed property to calculate the positions of differing characters
const differingPositions = computed(() => {
  const positions = [];
  const len = Math.min(inputText1.value.length, inputText2.value.length);

  for (let i = 0; i < len; i++) {
    if (inputText1.value[i] !== inputText2.value[i]) {
      positions.push(i + 1); // 1-based position
    }
  }

  return positions;
});
</script>
