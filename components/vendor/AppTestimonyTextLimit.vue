<template>
	<div class="h-10 w-10 relative">
		<svg class="transform -rotate-90" viewbox="0 0 150 150">
			<circle
				cx="90"
				cy="90"
				fill="none"
				stroke-width="12"
				:r="radius"
				class="stroke-current text-gray-300"
			/>

			<circle
				cx="90"
				cy="90"
				fill="none"
				stroke-width="12"
				:r="radius"
				class="stroke-current"
				:class=" percentageIsOver ? 'text-red-500' : 'text-blue-500' "
				:stroke-dasharray="dash"
				:stroke-dashoffset="offset"
			/>
		</svg>
	</div>
</template>

<script>
export default {
	data () {
		return {
			radius : 40
		}
	},

	props : {
		body : {
			required : true,
			type : String
		}
	},

	computed : {
		dash () {
			return 2 * Math.PI * this.radius
		},

		percentageIsOver () {
			return this.percentage > 100
		},

		percentage () {
			return Math.round((this.body.length * 100) / 500)
		},

		displayPercentage () {
			return this.percentage <= 100 ? this.percentage : 100
		},

		offset () {
			let circ = this.dash
			let progress = this.displayPercentage / 100
			return circ * (1 - progress)
		}
	}
}
</script>