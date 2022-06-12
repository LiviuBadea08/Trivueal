<template>
  <div
    class="w-screen h-screen absolute z-30 bg-black bg-opacity-60 flex justify-center items-center"
  >
    <div class="bg-green-700 p-4 text-center text-white">
      <p class="font-bold text-2x1">All Done!</p>
      <p class="my-4 font-bold text-3x1">100% score</p>

      <!--buttons-->
      <div class="flex">
        <div
          class="rounded-full py-1 w-28 border cursor-pointer hover:text-black hover:bg-white"
        >
          Done
        </div>
        <div
          class="rounded-full py-1 w-28 border cursor-pointer hover:text-black hover:bg-white"
        >
          Retry
        </div>
      </div>
    </div>
  </div>
</template>
