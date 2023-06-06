<template>
  <div class="bg-white p-12 rounded-lg shadow-lg w-full mt-8">
    <div v-if="idx < count">
      <p class="text-2xl font-bold">
        {{ questions[idx]["question"] }}
      </p>
      <label
        v-for="(answer, index) in questions[idx].answers"
        :key="index"
        :for="'' + index"
        class="block mt-4 border border-gray-300 rounded-lg py-2 px-6 text-lg"
        :class="{
          'hover:bg-gray-100 cursor-pointer': selectedAnswer == '',
          'bg-green-200': index == questions[idx].correctAnswer && selectedAnswer != '',
          'bg-red-200': selectedAnswer == index  
        }
        "
      >
        <input
          :id="'' + index"
          type="radio"
          class="hidden"
          :value="index"
          :disabled="selectedAnswer != ''"
          @change="answered($event)"
        >
        {{ answer }}
      </label>
      <div class="mt-6 flow-root">
        <button
          v-show="selectedAnswer != '' && idx < count - 1"
          class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2"
          @click="nextQuestion"
        >
          Next &gt;
        </button>
        <button
          v-show="selectedAnswer != '' && idx == count - 1"
          class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2"
          @click="showResults"
        >
          Finish
        </button>
      </div>
    </div>
    <div v-else>
      <h2 class="text-bold text-3xl">
        Results
      </h2>
      <div class="flex justify-start space-x-4 mt-6">
        <p>
          Correct Answers:
          <span class="text-2xl text-green-700 font-bold">{{
            correctAnswers
          }}</span>
        </p>
        <p>
          Wrong Answers:
          <span class="text-2xl text-red-700 font-bold">{{
            wrongAnswers
          }}</span>
        </p>
      </div>
      <div class="mt-6 flow-root">
        <button
          class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2"
          @click="resetQuiz"
        >
          Play again
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import tests from "~/data/tests.json";

const props = defineProps<{
  id: string;
}>();

const questions: any[] = ref(tests[props.id]);
const idx = ref(0)
const selectedAnswer = ref("")
const correctAnswers = ref(0)
const wrongAnswers = ref(0)
const count = ref(3)

const answered = (e) => {
    selectedAnswer.value = e.target.value
    if (selectedAnswer.value == questions[idx.value].correctAnswer) {
        correctAnswers.value = correctAnswers.value + 1
    } else {
        wrongAnswers.value = wrongAnswers.value + 1
    }
}

const nextQuestion = () => {
    idx.value = idx.value + 1
    selectedAnswer.value = ''
    document.querySelectorAll("input").forEach((el) => (el.checked = false));
}

const showResults = () => {
    idx.value = idx.value + 1
}

const resetQuiz = () => {
    idx.value = 0
    selectedAnswer.value = ""
    correctAnswers.value = 0
    wrongAnswers.value = 0
}

</script>

<style scoped></style>
