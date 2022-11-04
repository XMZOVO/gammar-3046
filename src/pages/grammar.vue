<script lang="ts" setup>

</script>

<template>
  <div grid="~ lg:cols-2 gap-2" h-full of-hidden>
    <div h-full grid="~ rows-[min-content_min-content_1fr]">
      <div font-bold py-4>
        文法查阅-3406
        <span> v1.0</span>
      </div>
      <div border="~ rounded" shadow>
        <input type="text" w-full p="x-6 y-4" border-none bg-transparent outline-none>
      </div>
      <div border="l b r" h-full mx-2>
        <div v-for="item in 8" :key="item" row text-left items-center px-3 py-2 gap-3 border="b" hover="bg-gray bg-op20">
          <div>item</div>
        </div>
      </div>
    </div>
  </div>
</template>
