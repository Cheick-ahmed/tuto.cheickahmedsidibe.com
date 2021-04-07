<template>
	<div class="flex flex-col items-center justify-center">
		<div class="relative transform -rotate-90">
			<div class="absolute transform rotate-90 space-x-1 inset-0 flex items-center justify-center">
				<span class="text-black text-lg font-semibold leading-tight tracking-wider">
					{{ progress }}%
				</span>
			</div>
			<svg viewBox="0 0 180 180" height="180" width="180">
				<circle
				cx="90"
				cy="90"
				:r="radius"
				fill="none"
				stroke-width="16"
				class="stroke-current text-blue-50"
				/>
				<circle
				cx="90"
				cy="90"
				:r="radius"
				fill="none"
				stroke-width="16"
				stroke-linejoin="round"
				stroke-linecap="round"
				class="stroke-current text-blue-700"
				:stroke-dasharray="dash"
				:stroke-dashoffset="offset"
				/>
			</svg>
		</div>
	</div>
</template>
<script>
export default {
	props : {
		progress : {
			type : Number,
			default : 30
		}
	},

	data () {
		return {
			radius : 80,
		}
	},
	computed : {
		dash () {
			return 2 * Math.PI * this.radius
		},
		offset () {
			let circ = this.dash
			let progress = this.progress / 100
			return circ * (1 - progress)
		}
	}
}
</script>