<script setup lang="ts">
import frame0 from "~/assets/frames/0.txt?raw";
import frame1 from "~/assets/frames/1.txt?raw";
import frame2 from "~/assets/frames/2.txt?raw";
import frame3 from "~/assets/frames/3.txt?raw";

const frames = [frame0, frame1, frame2, frame3];

const preWidth =
  Math.max(
    ...frames.map((f) => Math.max(...f.split("\n").map((l) => [...l].length))),
  ) + "ch";

const preHeight =
  Math.max(...frames.map((f) => f.split("\n").length)) + "em";

const currentFrame = ref(0);
let timer: ReturnType<typeof setTimeout>;

function runCycle() {
  let i = 0;
  function tick() {
    currentFrame.value = i;
    i++;
    if (i < frames.length) {
      timer = setTimeout(tick, 130);
    } else {
      timer = setTimeout(() => {
        currentFrame.value = 0;
        timer = setTimeout(runCycle, 2200);
      }, 130);
    }
  }
  tick();
}

onMounted(() => runCycle());
onUnmounted(() => clearTimeout(timer));
</script>

<template>
  <div class="text-[0.5rem] sm:text-xs md:text-sm lg:text-base" :style="{ width: preWidth, height: preHeight }">
    <pre class="logo">{{ frames[currentFrame] }}</pre>
  </div>
</template>

<style scoped>
.logo {
  font-family: monospace;
  line-height: 1;
  white-space: pre;
  user-select: none;
}
</style>
