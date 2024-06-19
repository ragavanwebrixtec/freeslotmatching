<template>
  <FreeSlot />
</template>

<script>
import FreeSlotMatching from "./components/FreeSlotMatching.vue";
export default {
  name: "App",
  components: {
    FreeSlot: FreeSlotMatching,
  },
};
</script>
