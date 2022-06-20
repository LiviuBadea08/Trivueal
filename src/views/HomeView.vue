/* eslint-disable prettier/prettier */
<template>
  <main class="flex h-screen item-center justify-center bg-gray-100">
    <QuizCompleteOverlay
      v-if="endOfQuiz"
      :percent="percentageScore"
      @restartQuiz="onQuizStart"
    ></QuizCompleteOverlay>
    <div
      class="overflow-hidden absolute bg-white flex-none container shadow-lg rounded-lg px-12 py-6"
    >
      <img
        src="@/assets/images/abstract.svg"
        alt=""
        class="absolute -top-10 left-0 object-none"
      />
      <div class="relative z-20">
        <div class="text-right text-gray-800">
          <p class="text-sm leading-3">Score</p>
          <p class="font-bold">{{ score }}</p>
        </div>

        <div class="bg-white shadow-lg p-1 rounded-full w-full h-5 mt-4">
          <div
            class="bg-blue-700 rounded-full w-11/12 h-full"
            :style="`width: ${timer}%`"
          ></div>
        </div>

        <div
          class="rounded-lg bg-gray-100 p-2 neumorph-1 text-center font-bold text-gray-800 mt-8"
        >
          <div class="bg-white p-5">
            {{ currentQuestion.question }}
          </div>
        </div>

        <div class="mt-8">
          <div v-for="(choice, item) in currentQuestion.choices" :key="item">
            <div
              class="neumorph-1 option-default bg-gray-100 p-2 rounded-lg mb-3 relative"
              :ref="optionChosen"
              @click="onOptionClicked(choice, item)"
            >
              <div
                class="bg-blue-700 p-1 transform rotate-45 rounded-md h-10 w-10 text-white font-bold absolute right-0 top-0 shadow-md"
              >
                <p class="transform -rotate-45">+10</p>
              </div>

              <div class="rounded-lg font-bold flex p-2">
                <div class="p-3 rounded-lg">{{ item }}</div>

                <div class="flex items-center pl-6">
                  {{ choice }}
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="mt-8 text-center">
          <div class="h-1 w-12 bg-gray-800 rounded-full mx-auto"></div>
          <p class="font-bold text-color-gray-800">
            {{ questionCounter }}/{{ questions.length }}
          </p>
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
import QuizCompleteOverlay from "./components/QuizCompleteOverlay.vue";
export default {
  setup() {
    let canClick = true;
    let timer = ref(100);
    let endOfQuiz = ref(false);
    let questionCounter = ref(0);
    let score = ref(0);
    const currentQuestion = ref({
      question: "",
      answer: 1,
      choices: [],
    });

    let percentageScore = ref(0);

    // eslint-disable-next-line
    const questions = ref([]);

    const loadQuestion = () => {
      canClick = true;
      if (questions.value.length > questionCounter.value) {
        timer.value = 100;
        currentQuestion.value = questions.value[questionCounter.value];
        console.log("Current question: " + currentQuestion.value);
        questionCounter.value++;
      } else {
        onQuizEnd();
        console.log("Out of questions");
      }
    };

    let itemsRef = [];
    const optionChosen = (element) => {
      if (element) {
        itemsRef.push(element);
      }
    };

    const clearSelected = (divSelected) => {
      setTimeout(() => {
        divSelected.classList.remove("option-correct");
        divSelected.classList.remove("option-wrong");
        divSelected.classList.add("option-default");
        loadQuestion();
      }, 1000);
    };

    const onOptionClicked = (choice, item) => {
      if (canClick) {
        const divContainer = itemsRef[item];
        const optionID = item + 1;
        if (currentQuestion.value.answer == optionID) {
          console.log("you are correct");
          score.value += 10;
          divContainer.classList.add("option-correct");
          divContainer.classList.remove("option-default");
        } else {
          console.log("you are wrong");
          divContainer.classList.add("option-wrong");
          divContainer.classList.remove("option-default");
        }
        timer.value = 100;
        canClick = false;
        clearSelected(divContainer);
        console.log(choice, item);
      } else {
        console.log("can't select question");
      }
    };

    const countdownTimer = function () {
      let interval = setInterval(() => {
        if (timer.value > 0) {
          timer.value--;
        } else {
          console.log("Time is up");
          onQuizEnd();
          clearInterval(interval);
        }
      }, 150);
    };

    const fetchQuestionsFromServer = async function () {
      console.log("Fetching questions from server...");
      fetch("https://opentdb.com/api.php?amount=10&category=27")
        .then((res) => {
          return res.json();
        })
        .then((data) => {
          const newQuestions = data.results.map((serverQuestion) => {
            const arrangedQuestion = {
              question: serverQuestion.question,
              choices: "",
              answer: "",
            };

            const choices = serverQuestion.incorrect_answers;

            arrangedQuestion.answer = Math.floor(Math.random() * 4 + 1);

            choices.splice(
              arrangedQuestion.answer - 1,
              0,
              serverQuestion.correct_answer
            );

            arrangedQuestion.choices = choices;

            return arrangedQuestion;
          });
          console.log("new formated questions", newQuestions);

          questions.value = newQuestions;
          loadQuestion();
          countdownTimer();
        });
    };

    const onQuizEnd = function () {
      percentageScore.value = (score.value / 100) * 100;

      timer.value = 0;

      endOfQuiz.value = true;
    };

    const onQuizStart = function () {
      canClick = true;
      timer.value = 100;
      endOfQuiz.value = false;
      questionCounter.value = 0;
      score.value = 0;
      currentQuestion.value = {
        question: "",
        answer: 1,
        choices: [],
      };

      percentageScore.value = 0;

      questions.value = [];

      fetchQuestionsFromServer();
    };

    onMounted(() => {
      fetchQuestionsFromServer();
    });

    return {
      timer,
      currentQuestion,
      questions,
      score,
      questionCounter,
      loadQuestion,
      onOptionClicked,
      optionChosen,
      endOfQuiz,
      onQuizEnd,
      percentageScore,
      onQuizStart,
    };
  },

  computed: {
    formattedQuestion() {
      let entities = {
        amp: "&",
        apos: "'",
        "#x27": "'",
        "#x2F": "/",
        "#39": "'",
        "#47": "/",
        lt: "<",
        gt: ">",
        nbsp: " ",
        quot: '"',
        "#039": "'",
      };
      return this.currentQuestion.question.replace(
        /&([^;]+);/gm,
        function (match, entity) {
          return entities[entity] || match;
        }
      );
    },
  },
  components: {
    QuizCompleteOverlay,
  },
};
</script>
