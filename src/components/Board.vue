<template>
	<div class="board">
		<div :style="{height: '100px'}">
			<h2 v-if="gameStatus.gameOver">{{gameStatus.gameOver ? 'Game Over!': ''}} {{gameStatus.winner ? `Winner: ${gameStatus.winner}` : ''}}</h2>
			<h2 v-else>Next: <span :style="{fontSize: '36px'}">{{xIsNext ? 'X' : 'O' }}</span></h2>
		</div>
		<div class="board-row">
			<Square v-for="(square) in board.slice(0,3)" 
			:key=square.index 
			:value="square.value" 
			:index="square.index"
			:winningTiles="gameStatus.winningTiles"
			@clicked="gameStatus.gameOver ? null : updateBoard(square.index)"
			/>
		</div>
		<div class="board-row">
			<Square v-for="(square) in board.slice(3,6)" 
			:key=square.index 
			:value="square.value" 
			:index="square.index"
			:winningTiles="gameStatus.winningTiles"
			@clicked="gameStatus.gameOver ? null : updateBoard(square.index)"
			/>
		</div>
		<div class="board-row">
			<Square v-for="(square) in board.slice(6,9)" 
			:key=square.index 
			:value="square.value" 
			:index="square.index"
			:winningTiles="gameStatus.winningTiles"
			@clicked="gameStatus.gameOver ? null : updateBoard(square.index)"
			/>
		</div>
		<div class="restart" v-if="gameStatus.gameOver" @click="resetBoard">Restart</div>
	</div>
</template>

<script>
	import Square from './Square'

	function initialBoard() {
		return {
			xIsNext: true,
			gameStatus: {
				gameOver: false,
				winner: false,
				winningTiles: []
			},
			// Board initial state: [{index: 0, value: ''}, ...]
			board: [0,1,2,3,4,5,6,7,8].map((x) => {return {index: x, value: ''}})
		}
	}

	function checkIfBoardFull(board=this.board) {
		const values = board.map(x => x.value)
		const full = values.some(x => x === '')
		return !full
	}

	function checkWinner(board=this.board){
		// Square indexes of potential winning combinations.
		const winningCombos = [
			[0,1,2],
			[3,4,5],
			[6,7,8],
			[0,3,6],
			[1,4,7],
			[2,5,8],
			[0,4,8],
			[2,4,6]
		]

		// Check all combos for 'X,X,X' or 'O,O,O'
		const rows = winningCombos.map( arr => Array.from(arr, x => board[x].value ))
		const winningIndex = rows.findIndex(e => e.join() === 'X,X,X' || e.join() === 'O,O,O')
		return winningCombos[winningIndex] || []
	}

	export default {
		name: 'Board',
		components: {
			Square
		},
		data: function() {
			return initialBoard()
		},
		beforeUpdate: function() {
			const winningTiles = checkWinner(this.board)
			if (winningTiles.length > 0) {
				this.gameStatus.gameOver = true;
				this.gameStatus.winner = !this.xIsNext ? 'X' : 'O';
				this.gameStatus.winningTiles = winningTiles;
			}
			else {
				this.gameStatus.gameOver = checkIfBoardFull(this.board)
			}
		},
		methods: {
			updateBoard(i) {
				if (this.board[i].value === '') {
					const mark = this.xIsNext ? 'X' : 'O'
					this.board[i].value = mark
					this.xIsNext = !this.xIsNext
				}
			},
			resetBoard() {
				Object.assign(this.$data, initialBoard())
			}
		}
	}

</script>

<style>
.board {
	width: 60%;
	margin-left: auto;
}

.board-row:after {
  clear: both;
  content: "";
  display: table; 
}

.restart {
	text-align: center;
	background: #97e864;
	border-radius: 3px;
	margin-top: 20px;
	float: left;
	padding: 0.4em;
	font-size: 1.5em;
}
</style>