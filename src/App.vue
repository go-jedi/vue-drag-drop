<template>
  <div>
    <!-- <DragCard /> -->
    <!-- <DragTrello /> -->
    <DragUpload />
  </div>
</template>

<script lang="js">
import { defineComponent } from 'vue'

import DragCard from "./components/DragCard/DragCard.vue"
import DragTrello from "./components/DragTrello/DragTrello.vue"
import DragUpload from "./components/DragUpload/DragUpload.vue"

export default defineComponent({
  name: "App",
  setup() {
    return {}
  },
  components: {
    DragCard,
    DragTrello,
    DragUpload
  }
})
</script>

<style lang="css" scoped></style>
