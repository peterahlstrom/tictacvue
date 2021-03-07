<template>
	<button 
		class="square" 
		:class="classObject"
		@click="$emit('clicked', index)"

		>{{ value }}</button>
</template>

<script>
	export default {
		name: 'Square',
		props: {
			value: String,
			index: Number,
			winningTiles: Array
		},
		computed: {
			classObject: function() {
				return {
					'markedTile': this.winningTiles.includes(this.index)
				}

			}
		}
	}
</script>

<style scope>
.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 100px;
  font-weight: bold;
  line-height: 100px;
  height: 100px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 100px;
}

.square:focus {
  outline: none;
}

.square:focus {
  background: #ddd;
}

.markedTile, .markedTile:focus {
	background: #97e864;
}
</style>