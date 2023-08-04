<template>
  <div class="app">
    <div
      class="drop-area"
      v-if="drag === true"
      @dragstart="(e) => dragStartHandler(e)"
      @dragleave="(e) => dragLeaveHandler(e)"
      @dragover="(e) => dragStartHandler(e)"
      @drop="(e) => onDropHandler(e)">
      Отпустите файлы, для загрузить их
    </div>
    <div
      v-if="drag === false"
      @dragstart="(e) => dragStartHandler(e)"
      @dragleave="(e) => dragLeaveHandler(e)"
      @dragover="(e) => dragStartHandler(e)">
      Перетащите файлы, чтобы загрузить их
    </div>
  </div>
</template>

<script lang="js">
import { defineComponent, ref } from 'vue'

export default defineComponent({
    name: "DragUploadComponent",
    setup () {
        const drag = ref(false)

        const dragStartHandler = (event) => {
            try {
                event.preventDefault()
                drag.value = true
            } catch (error) {
                console.log("error in dragStartHandler -->", error)
            }
        }

        const dragLeaveHandler = (event) => {
            try {
                event.preventDefault()
                drag.value = false
            } catch (error) {
                console.log("error in dragLeaveHandler -->", error)
            }
        }

        const onDropHandler = (event) => {
            try {
                event.preventDefault()
                let files = [...event.dataTransfer.files]
                const formData = new FormData()
                formData.append("files", files)
                drag.value = false
            } catch (error) {
                console.log("error in onDropHandler -->", onDropHandler)
            }
        }

        return {
            // ref
            drag,
            // func
            dragStartHandler,
            dragLeaveHandler,
            onDropHandler
        }
    }
})
</script>

<style lang="css" scoped>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.app {
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 40px;
}

.drop-area {
  width: 80vw;
  height: 80vh;
  border: 5px dashed black;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
