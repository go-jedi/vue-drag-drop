<template>
  <div class="app">
    <div v-for="card in cardList.sort(sortCards)" :key="card.id">
      <div
        class="card"
        draggable="true"
        @dragstart="dragStartHandler(card)"
        @dragleave="dragEndHandler"
        @dragend="dragEndHandler"
        @dragover.prevent="dragOverHandler"
        @drop.prevent="(event) => dropHandler(event, card)">
        {{ card.text }}
      </div>
    </div>
  </div>
</template>

<script lang="js">
import { defineComponent, ref } from 'vue'

export default defineComponent({
    name:"DragCardComponent",
    setup() {
    const cardList = ref([
      { id: 1, order: 3, text: "КАРТОЧКА 3" },
      { id: 2, order: 1, text: "КАРТОЧКА 1" },
      { id: 3, order: 2, text: "КАРТОЧКА 2" },
      { id: 4, order: 4, text: "КАРТОЧКА 4" },
    ])
    const currentCard = ref(null)

    const dragStartHandler = (needCard) => {
      try {
        currentCard.value = needCard
      } catch (error) {
        console.log("error in dragStartHandler -->", error)
      }
    }

    const dragEndHandler = (event) => {
      try {
        event.target.style.background = "white";
      } catch (error) {
        console.log("error in dragEndHandler -->", error)
      }
    }

    const dragOverHandler = (event) => {
      try {
        event.target.style.background = "lightgray";
      } catch (error) {
        console.log("error in dragOverHandler -->", error)
      }
    }

    const dropHandler = (event, needCard) => {
      try {
        cardList.value = cardList.value.map((e) => {
          if (e.id === needCard.id) {
            return {...e, order: currentCard.value.order}
          }
          if (e.id === currentCard.value.id) {
            return {...e, order: needCard.order}
          }
          return e;
        })
        event.target.style.background = "white";
      } catch (error) {
        console.log("error in dropHandler -->", error)
      }
    }

    const sortCards = (a, b)=> {
      try {
        if (a.order > b.order) {
          return 1
        } else {
          return -1
        }
      } catch (error) {
        console.log("error in sortCards -->", error)
      }
    }

    return {
      // ref
      cardList,
      // func
      dragStartHandler,
      dragEndHandler,
      dragOverHandler,
      dropHandler,
      sortCards,
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
}

.card {
  width: 200px;
  height: 300px;
  border-radius: 12px;
  border: 5px solid;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px;
  cursor: grab;
}
</style>
