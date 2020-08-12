<template>
  <section class="game-section">
    <div class="game-container">
      <h1>Pussel</h1>
      <div 
        class="items-container" 
        :class="{shuffle: isShuffling}"
      >
        <div
          v-for="(item, index) in items"
          :key="index"
          class="item"
          :class="{empty : item.nr === 16, clickable: clickableHandler(index)}"
          @click="isStarted ? validateItem(index) : ''"
        >
          <p>{{ !isShuffling ? item.nr : "" }}</p>
        </div>
      </div>
      <div class="msg-container">
        <p v-if="checkOrder() && isStarted">
          {{ message.win }}
        </p>
      </div>
      <div class="btn-container">
        <button @click="startGame()">
          <p v-if="!isStarted">
            Starta Spel
          </p>
          <p v-if="isStarted && !checkOrder()">
            Avsluta Spel
          </p>
          <p v-if="isStarted && checkOrder()">
            Spela Igen?
          </p>
        </button>
        <button v-if="isStarted" @click="shuffleItems(items)">
          <p>Slumpa</p>
        </button>
      </div>
    </div>
  </section>
</template>
<style lang="scss" scoped>
.game-section {
  width: 100%;
  height: 100vh;
  background: $background;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  .game-container {
    width: 40%;
    background: $thirdary;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border-radius: 15px;
    box-shadow: $shadow-hover;
    @include breakpoint(xsmax) {
      width: 100%;
    }
    @include breakpoint(xsmin) {
      width: 80%;
    }
    @include breakpoint(smed) {
      width: 40%;
    }
    @include breakpoint(smed) {
      width: 35%;
    }
    @include breakpoint(lg) {
      width: 25%;
    }
    h1 {
      margin-top: 1rem;
    }
    .btn-container {
      button {
        transition: 250ms;
        outline: none;
        background: $primary;
        border: none;
        border-radius: 10px;
        margin: {
          bottom: 2rem;
          left: 0.2rem;
          right:0.2rem;
        }
        box-shadow: $shadow;
        p {
          padding: 1rem;
          color: white;
        }
        &:hover {
          box-shadow: none;
        }
      }
    }
    .items-container {
      display: flex;
      align-items: center;
      justify-content: center;
      display: flex;
      flex-wrap: wrap;
      width: 100%;
      padding-bottom: 100%;
      position: relative;
      :first-child {
        top: 8%;
        left: 8%;
      }
      :nth-child(2) {
        top: 8%;
        left: 29.33%;
      }
      :nth-child(3) {
        top: 8%;
        left: 50.66%;
      }
      :nth-child(4) {
        top: 8%;
        left: 71.99%;
      }
      :nth-child(5) {
        top: 29.33%;
        left: 8%;
      }
      :nth-child(6) {
        top: 29.33%;
        left: 29.33%;
      }
      :nth-child(7) {
        top: 29.33%;
        left: 50.66%;
      }
      :nth-child(8) {
        top: 29.33%;
        left: 71.99%;
      }
      :nth-child(9) {
        top: 50.66%;
        left: 8%;
      }
      :nth-child(10) {
        top: 50.66%;
        left: 29.33%;
      }
      :nth-child(11) {
        top: 50.66%;
        left: 50.66%;
      }
      :nth-child(12) {
        top: 50.66%;
        left: 71.99%;
      }
      :nth-child(13) {
        top: 71.99%;
        left: 8%;
      }
      :nth-child(14) {
        top: 71.99%;
        left: 29.33%;
      }
      :nth-child(15) {
        top: 71.99%;
        left: 50.66%;
      }
      :nth-child(16) {
        top: 71.99%;
        left: 71.99%;
      }
      .item,
      .empty {
        position: absolute;
        background-color: $secondary;
        display: flex;
        align-items: center;
        justify-content: center;
        width: 20%;
        height: 20%;
        border-radius: 15px;
        transition: 250ms;
        p {
          text-align: center;
        }
      }
      .empty {
        opacity: 0;
      }
      .clickable {
        background-color: $hover;
        box-shadow: $shadow;
        p {
          color: white;
        }
        &:hover {
          box-shadow: none;
          background-color: $secondary;
          p {
            color: $text;
          }
        }
      }
    }
    .shuffle {
      .item {
        top: 40%;
        left: 40%;
      }
      .clickable {
        background-color: $secondary;
        box-shadow: none;
      }
    }
  }
}
</style>
<script>
export default {
  data() {
    return {
      isShuffling: false,
      message: {
        win: 'Grattis, du klarade pusslet!',
      },
      isStarted: false,
      items: [
        { nr: 1 },
        { nr: 2 },
        { nr: 3 },
        { nr: 4 },
        { nr: 5 },
        { nr: 6 },
        { nr: 7 },
        { nr: 8 },
        { nr: 9 },
        { nr: 10 },
        { nr: 11 },
        { nr: 12 },
        { nr: 13 },
        { nr: 14 },
        { nr: 15 },
        { nr: 16 },
      ],
    }
  },
  mounted() {
    this.clickableHandler()
  },
  methods: {
    // Starts game and triggers shuffleItems()
    startGame() {
      this.shuffleItems(this.items)
      if (this.checkOrder() && this.isStarted) {
        this.shuffleItems(this.items)
        return this.isStarted = true  
      }
      this.isStarted = !this.isStarted
    },
    // Handles clickable classes
    clickableHandler(clickableIndex) {
      // Returns true if a item is clickable and toggles the class
      for (let index = 0; index < this.items.length; index++) {
        if(!this.isStarted) {
          return false
        }
        if(this.checkOrder()) {
          return true
        }
        if (this.items[index].nr === 16) {
          if (
            (index !== 0 && index !== 4 && index !== 8 && index !== 12) &&
            (this.items[clickableIndex + 1] && this.items[clickableIndex + 1].nr === 16)
          ) {
            return true
          }
          if (
            (index !== 3 && index !== 7 && index !== 11 && index !== 15) &&
            (this.items[clickableIndex - 1] && this.items[clickableIndex - 1].nr === 16)  
          ) {
            return true
          }
        }
        if (
          (this.items[clickableIndex + 4] && this.items[clickableIndex + 4].nr === 16) ||
          (this.items[clickableIndex - 4] && this.items[clickableIndex - 4].nr === 16)
        ) {
          return true
        } 
      }
      return false
    },
    // Shuffles the array
    shuffleItems(items) {
      for (var i = items.length - 1; i >= 0; i--) {
        let random = Math.floor(Math.random() * i + 1)
        let temp = items[random]
        items[random] = items[i]
        items[i] = temp
      }
      this.items = null
      this.items = items
      // Toggles class
      this.isShuffling = true
      setTimeout(() => {
        this.isShuffling = false
      }, 500)

    },
    // Checks if the clicked item is valid 
    validateItem(clickedIndex) {
      if(this.checkOrder()) {
        return false
      }
      // Checks for empty slot
      for (let index = 0; index < this.items.length; index++) {
        // Checks if the closest item is on the same row
        if (this.items[index].nr === 16) {
          // Handles movement to left and right
          if (
            (index !== 0 && index !== 4 && index !== 8 && index !== 12) &&
            (this.items[clickedIndex + 1] && this.items[clickedIndex + 1].nr === 16)
          ) {
            if (this.items[index] && this.items[index].nr === 16) {
              this.moveItem(clickedIndex, index)
            }
          }
          if (
            (index !== 3 && index !== 7 && index !== 11 && index !== 15) &&
            (this.items[clickedIndex - 1] && this.items[clickedIndex - 1].nr === 16)  
          ) {
            if (this.items[index] && this.items[index].nr === 16) {
              this.moveItem(clickedIndex, index)
            }
          }
        }
        // Handles up and down movement
        if (
          (this.items[clickedIndex + 4] && this.items[clickedIndex + 4].nr === 16) ||
          (this.items[clickedIndex - 4] && this.items[clickedIndex - 4].nr === 16)
        ) {
          if (this.items[index] && this.items[index].nr === 16) {
            this.moveItem(clickedIndex, index)
          }
        }
      } 
    },
    // Changes value of the clicked index and the empty slot to make the items move
    moveItem(clickedIndex, lastIndex) {
      this.items.splice(lastIndex, 1, {nr: this.items[clickedIndex].nr})
      this.items.splice(clickedIndex, 1, {nr: 16})
    },
    // Checks if items is sorted and returns true if it is
    checkOrder() {
      for (let i = 0; i < this.items.length - 1; i++) {
        if (this.items[i].nr > this.items[i+1].nr) {
          return false
        } 
      }
      return true
    }
  },
}
</script>
