<script setup>
import { ref, computed } from 'vue'

const player = ref('X')
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
])
const moves = ref(0)
const draw = ref(false)

const CalculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }

  if (moves.value === 9) {
    draw.value = true
  }
  return null;
}

const winner = computed(() => CalculateWinner(board.value.flat()))

const MakeMove = (x, y) => {
  if (winner.value) return
  
  if (board.value[x][y] !== '') return

  moves.value++;

  console.log(moves.value)

  board.value[x][y] = player.value

  player.value = player.value === 'X' ? 'O' : 'X'
}

const ResetGame = () => {
  board.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ]

  player.value = 'X'
  moves.value = 0
  draw.value = false
}
</script>

<template>
  <main class="flex flex-col items-center justify-center p-8 text-center dark:bg-gray-800 min-h-screen dark:text-white">
    <p class="text-xl mb-4">Player {{ player }}'s turn</p>

    <div class="flex flex-col items-center mb-8">
      <div class="border border-white">
        <div 
          v-for="(row, x) in board" 
          :key="x" 
          class="flex"
        >
          <div 
            v-for="(cell, y) in row" 
            :key="y"
            @click="MakeMove(x, y)"
            :class="`border border-white w-20 h-20 hover:bg-gray-700 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer duration-200 ${ cell === 'X' ? 'text-pink-400' : 'text-blue-400'}`"
          >
          {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
          </div>
        </div>
      </div>
    </div>

    <button @click="ResetGame" class="px-4 py-2 bg-blue-600 rounded uppercase font-bold hover:bg-blue-700 duration-200 mb-8">Reset</button>

    <p class="text-6xl font-bold" style="height: 80px">
      <span v-if="winner && !draw">
        Player {{ winner }} wins!
      </span>
      <span v-if="draw">
        Draw!
      </span>
    </p>
</main>
</template>

<style scoped>
</style>