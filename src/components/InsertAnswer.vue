<template>
  <div class="border-spacing-1 border-2 border-gray-800 p-3 w-3/4 md:w-1/2">
    <h1 class="justify-center items-center flex font-extrabold text-xl">
      Input Jawaban
    </h1>
    <label class="form-control">
      <div class="label">
        <span class="label-text">Jawaban yang benar</span>
        <span class="label-text-alt">Jumlah soal {{ characterCount1 }}</span>
      </div>
      <input
        v-model="inputText1"
        placeholder="Input kunci Jawaban..."
        class="input input-bordered"
        :disabled="disableInputText1"
      />
      <button
        class="btn btn-primary mt-3"
        :disabled="inputText1.length === 0"
        @click="handleButtonClick"
      >
        {{ disableInputText1 ? "Buka" : "Kunci" }} Jawaban
      </button>
      <!-- Input Nopes -->
      <div class="label">
        <span class="label-text">No Pendaftaran</span>
      </div>
      <input
        v-model="formattedNumber"
        :mask="numberMask"
        placeholder="XX-XX-XXX"
        class="input input-bordered"
        @input="handleNumberInput"
        maxlength="7"
      />
      <!-- Input Siswa -->
      <div class="label">
        <span class="label-text">Hasil jawaban siswa</span>
      </div>
      <input
        v-model="inputText2"
        placeholder="Input Hasil siswa..."
        class="input input-bordered"
        v-bind:maxlength="characterCount1"
      />
      <button
        class="btn btn-primary mt-3"
        @click="simpan"
        :disabled="(inputText2.length === 0, formattedNumber.length === 0)"
      >
        Simapan
      </button>
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
import Swal from "sweetalert2";

// Reactive variables to store the input texts
const inputText1 = ref("");
const inputText2 = ref("");
const formattedNumber = ref("");
// Reactive variables to store the processed texts
const processedText1 = ref("");
const processedText2 = ref("");
const disableInputText1 = ref(false);
// Watch for changes in the inputText1 variable

const simpan = () => {
  if (!inputText1.value) {
    Swal.fire({
      title: "Simpan",
      text: "Data gagal Disimpan karena jawaban yang benar harus disi",
      icon: "warning",
    });
    return;
  }
  if (!inputText2.value) {
    Swal.fire({
      title: "Simpan",
      text: "Data gagal Disimpan karena jawaban siswa kosong",
      icon: "warning",
    });
    return;
  }
  if (!formattedNumber.value) {
    Swal.fire({
      title: "Simpan",
      text: "Data gagal Disimpan karena No Peserta Kosong",
      icon: "warning",
    });
    return;
  }

  Swal.fire({
    title: "Simpan",
    text: "Data Berhasil Disimpan",
    icon: "success",
  });
};
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
const handleButtonClick = () => {
  // Your button click logic here
  disableInputText1.value = !disableInputText1.value;
};
const handleNumberInput = (event) => {
  const inputValue = event.target.value;
  // Remove non-numeric characters from the input
  const numericValue = inputValue.replace(/\D/g, "");

  // Format the number using the desired pattern
  formattedNumber.value = numericValue.replace(
    /(\d{2})(\d{2})(\d{3})/,
    "$1-$2-$3"
  );
};
</script>
