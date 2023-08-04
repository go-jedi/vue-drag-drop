<template>
  <div class="app">
    <div v-for="board in boards" :key="board.id">
      <div
        class="board"
        @dragover.prevent="(event) => dragOverHandler(event)"
        @drop.prevent="(event) => dropCardHandler(event, board)">
        <div class="board__title">{{ board.title }}</div>
        <div
          class="item"
          draggable="true"
          @dragstart="(event) => dragStartHandler(event, board, item)"
          @dragleave="(event) => dragLeaveHandler(event)"
          @dragend="(event) => dragEndHandler(event)"
          @dragover.prevent="(event) => dragOverHandler(event)"
          @drop.prevent="(event) => dropHandler(event, board, item)"
          v-for="item in board.items"
          :key="item.id">
          {{ item.title }}
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="js">
import { defineComponent, ref } from 'vue'

export default defineComponent({
    name: "DragTrelloComponent",
    setup () {
        const boards = ref([
            {id: 1, title: "Сделать", items: [{id: 1, title: "Пойти в магазин"}, {id: 2, title: "Выкинуть мусор"}, {id: 3, title: "Покушать"}]},
            {id: 2, title: "Проверить", items: [{id: 4, title: "Код ревью"}, {id: 5, title: "Задачи на факториал"}, {id: 6, title: "Задачи на фибоначчи"}]},
            {id: 3, title: "Сделано", items: [{id: 7, title: "Снять видео"}, {id: 8, title: "Смонтировать"}, {id: 9, title: "Отрендерить"}]},
        ])
        const currentBoard = ref(null)
        const currentItem = ref(null)

        const dragStartHandler = (event, board, item) => {
            try {
                currentBoard.value = board
                currentItem.value = item
            } catch (error) {
                console.log("error in dragStartHandler -->", error);
            }
        }

        const dragLeaveHandler = (event) => {
            try {
                event.target.style.boxShadow = "none";
            } catch (error) {
                console.log("error in dragLeaveHandler -->", error);
            }
        }

        const dragEndHandler = (event) => {
            try {
                event.target.style.boxShadow = "none";
            } catch (error) {
                console.log("error in dragEndHandler -->", error);
            }
        }

        const dragOverHandler = (event) => {
            try {
                if (event.target.className == "item") {
                    event.target.style.boxShadow = "0 2px 3px gray";
                }
            } catch (error) {
                console.log("error in dragOverHandler -->", error);
            }
        }

        const dropHandler = (event, board, item) => {
            try {
                if (board.id ===currentBoard.value.id) {
                    const currentIndex = currentBoard.value.items.indexOf(currentItem.value)
                    currentBoard.value.items.splice(currentIndex, 1)
                    const dropIndex = board.items.indexOf(item)
                    board.items.splice(dropIndex + 1, 0, currentItem.value)
                    boards.value.map((e) =>{
                        if (e.id === board.id) {
                            return board
                        }
                        if (e.id === currentBoard.value.id) {
                            return currentBoard.value
                        }
                        return e;
                    })
                }
                event.target.style.boxShadow = "none"
            } catch (error) {
                console.log("error in dropHandler -->", error)
            }
        }

        const dropCardHandler = (event, board) => {
            try {
                if (board.id !==currentBoard.value.id) {
                    board.items.push(currentItem.value)
                    const currentIndex = currentBoard.value.items.indexOf(currentItem.value)
                    currentBoard.value.items.splice(currentIndex, 1)
                    boards.value.map((e) =>{
                        if (e.id === board.id) {
                            return board
                        }
                        if (e.id === currentBoard.value.id) {
                            return currentBoard.value
                        }
                        return e;
                    })
                }
                event.target.style.boxShadow = "none"
            } catch (error) {
                console.log("error in dropCardHandler -->", error)
            }
        }

        return {
            // ref
            boards,
            // func
            dragStartHandler,
            dragLeaveHandler,
            dragEndHandler,
            dragOverHandler,
            dropHandler,
            dropCardHandler,
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

.board {
  min-width: 300px;
  min-height: 400px;
  border: 5px solid lightgray;
  padding: 20px 10px;
  border-radius: 12px;
  margin: 10px;
  display: flex;
  align-items: center;
  flex-direction: column;
}

.board__title {
  font-size: 1.5rem;
  font-weight: 700;
}

.item {
  width: 100%;
  border: 2px solid lightpink;
  padding: 10px;
  border-radius: 6px;
  margin: 5px 0;
  cursor: grab;
  background-color: #fcfcf9;
}
</style>
