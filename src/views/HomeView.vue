<template>
  <main class="flex h-screen item-center justify-center bg-gray-100">
    <!-- quiz container -->
    <div
      class="overflow-hidden absolute bg-white flex-none container shadow-lg rounded-lg px-12 py-6"
    >
      <img
        src="@/assets/images/abstract.svg"
        alt=""
        class="absolute -top-10 left-0 object-none"
      />
      <!-- contents -->
      <div class="relative z-20">
        <!-- score container -->

        <div class="text-right text-gray-800">
          <p class="text-sm leading-3">Score</p>
          <p class="font-bold">60</p>
        </div>

        <!-- timer container -->
        <div class="bg-white shadow-lg p-1 rounded-full w-full h-5 mt-4">
          <div class="bg-blue-700 rounded-full w-11/12 h-full"></div>
        </div>

        <!-- question container -->
        <div
          class="rounded-lg bg-gray-100 p-2 neumorph-1 text-center font-bold text-gray-800 mt-8"
        >
          <div class="bg-white p-5">
            {{ currentQuestion.question }}
          </div>
        </div>

        <!-- option container -->

        <div class="mt-8">
          <!-- option container -->
          <div v-for="(choice, item) in currentQuestion.choices" :key="item">
            <div
              class="neumorph-1 bg-gray-100 p-2 rounded-lg mb-3"
              :ref="optionChosen"
              @click="onOptionClicked(choice, item)"
            >
              <div class="bg-white rounded-lg font-bold flex p-2">
                <!-- option id -->
                <div class="bg-gray-400 p-3 rounded-lg">{{ item }}</div>

                <!-- option name -->

                <div class="flex items-center pl-6">
                  {{ choice }}
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- progress indicator container-->
        <div class="mt-8 text-center">
          <div class="h-1 w-12 bg-gray-800 rounded-full mx-auto"></div>
          <p class="font-bold text-color-gray-800">2/10</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.neumorph-1 {
  box-shadow: 6px 6px 18px rgba(0, 0, 0, 0.09), -6px -6px 18px #ffffff;
}
.container {
  max-width: 400px;
  border-radius: 25px;
}
</style>

<script>
import { ref, onMounted } from "vue";

export default {
  setup() {
    // data
    let questionCounter = ref(0);
    const currentQuestion = ref({
      question: "",
      answer: 1,
      /* eslint-disable */
      choices: [],
      /* eslint-enable */
    });

    const questions = [
      {
        question: "Quien chupa mas vergoglios?",
        answer: 1,
        choices: ["Diegu gallu", "Diegu", "Dieguin", "D"],
      },
      {
        question: "Quien come mas colitas?",
        answer: 1,
        choices: ["Diegu gallu", "Diegu", "Dieguin", "D"],
      },
    ];

    const onQuizStart = () => {
      currentQuestion.value = questions[questionCounter.value];
    };

    // methods/functions
    let itemsRef = [];
    const optionChosen = (element) => {
      if (element) {
        itemsRef.push(element);
      }
    };
    const onOptionClicked = (choice, item) => {
      const divContainer = itemsRef[item];
      const optionID = item + 1;
      if (currentQuestion.value.answer == optionID) {
        console.log("you are correct");
        divContainer.classList.add("option-correct");
        divContainer.classList.remove("option-default");
      } else {
        console.log("you are wrong");
        divContainer.classList.add("option-wrong");
        divContainer.classList.remove("option-default");
      }
      console.log(choice, item);
    };

    // lifecycle hooks

    onMounted(() => {
      onQuizStart();
    });

    // return
    return {
      currentQuestion,
      questions,
      questionCounter,
      onQuizStart,
      onOptionClicked,
      optionChosen,
    };
  },
};
</script>
