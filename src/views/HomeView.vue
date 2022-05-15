<template>
  <main>
    <div class="bg-white container shadow-lg rounded-lg px-12 py-6">
      <!-- question container -->
      <div
        class="rounded-lg bg-gray-100 p-2 neumorph-1 text-center font-bold text-gray-800"
      >
        <div class="bg-white p-5">QUIEN ES EL MAS CHUPAPOLLAS DEL MUNDO?</div>
      </div>

      <!-- option container -->

      <div class="mt-8">
        <!-- option container -->
        <div class="neumorph-1 bg-gray-100 p-2 rounded-lg mb-3">
          <div class="bg-white rounded-lg font-bold flex p-2">
            <!-- option id -->
            <div class="bg-gray-400 p-3 rounded-lg">A</div>

            <!-- option name -->

            <div class="flex items-center pl-6">Diegu</div>
          </div>
        </div>

        <div class="neumorph-1 bg-gray-100 p-2 rounded-lg mb-3">
          <div class="bg-red-600 rounded-lg font-bold flex p-2 text-white">
            <!-- option id -->
            <div class="bg-gray-800 p-3 rounded-lg">B</div>

            <!-- option name -->

            <div class="flex items-center pl-6">Diegu</div>
          </div>
        </div>

        <div class="neumorph-1 bg-gray-100 p-2 rounded-lg mb-3 relative">
          <div
            class="bg-blue-700 p-1 transform rotate-45 rounded-md h-10 w-10 text-white font-bold absolute right-0 top-0 shadow-md"
          >
            <p class="transform -rotate-45">+10</p>
          </div>
          <div class="bg-green-700 rounded-lg font-bold flex p-2 text-white">
            <!-- option id -->
            <div class="bg-gray-800 p-3 rounded-lg">C</div>

            <!-- option name -->

            <div class="flex items-center pl-6">Diegu el lambechotas</div>
          </div>
        </div>

        <!-- option container -->
        <div class="neumorph-1 bg-gray-100 p-2 rounded-lg mb-3">
          <div class="bg-white rounded-lg font-bold flex p-2">
            <!-- option id -->
            <div class="bg-gray-400 p-3 rounded-lg">D</div>

            <!-- option name -->

            <div class="flex items-center pl-6">Diegu</div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.neumorph-1 {
  box-shadow: 6px 6px 18px rgba(0, 0, 0, 0.09), -6px -6px 18px #ffffff;
}
</style>
