<template>
	<div class="flex items-center px-4 py-5 rounded" :class="bg" v-if="!hidden">
		<template v-if="loading">
			<div class="loader ease-linear rounded-full border-8 border-t-8 border-gray-500 h-8 w-8 mr-3"></div>
		</template>
		<template v-if="icon != 'none'">
			<template v-if="icon == 'info'">
				<div class="mr-3" :class="text">
					<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24" class="fill-current"><path class="heroicon-ui" d="M12 22a10 10 0 1 1 0-20 10 10 0 0 1 0 20zm0-2a8 8 0 1 0 0-16 8 8 0 0 0 0 16zm0-9a1 1 0 0 1 1 1v4a1 1 0 0 1-2 0v-4a1 1 0 0 1 1-1zm0-4a1 1 0 1 1 0 2 1 1 0 0 1 0-2z"/></svg>
				</div>
			</template>
		</template>
		<div class="flex w-full justify-between">
			<div class="font-bold text-sm" :class="text">
				{{ content }}
				<slot />
			</div>
			<a href="#" class="font-bold" :class="text" @click.prevent="close">
				<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24" class="fill-current"><path class="heroicon-ui" d="M16.24 14.83a1 1 0 0 1-1.41 1.41L12 13.41l-2.83 2.83a1 1 0 0 1-1.41-1.41L10.59 12 7.76 9.17a1 1 0 0 1 1.41-1.41L12 10.59l2.83-2.83a1 1 0 0 1 1.41 1.41L13.41 12l2.83 2.83z"/></svg>
			</a>
		</div>
	</div>
</template>

<script>
	import { EventBus } from '~/plugins/event-bus'
	export default {
		transation: 'showField',
		data() {
			return {
				themes: {
					primary: { bg: 'bg-blue-400', text: 'text-white' },
					secondary: { bg: 'bg-gray-200', text: 'text-gray-600' },
				},
				hidden: false,
				timePassed: 0,
      			timerInterval: null
			}
		},
		props: {
			content: {
				type: String,
				required: false,
				default: ''
			},
			loading: {
				type: Boolean,
				required: false,
				default: false
			},
			theme: {
				type: String,
				required: false,
				default: 'secondary'
			},
			id: {
				type: String,
				required: false,
				default: ''
			},
			timing: {
				type: Number,
				required: false,
				default: 0
			},
			icon: {
				type: String, 
				required:false,
				default: 'none'
			}
		},
		computed: {
			bg() {
				return this.resolveTheme().bg || ''
			},
			text() {
				return this.resolveTheme().text || ''
			},
			timeLeft() {
		      return this.timing - this.timePassed
		    },
		},
		watch: {
		    timeLeft(newValue) {
		    	if (newValue === 0) {
		        	this.onTimesUp()
		      	}
		    }
		},
		methods: {
			resolveTheme() {
				return this.themes[this.theme]
			},
			close() {
				this.hidden = true
				if (this.id) {
					EventBus.$emit('alert:closed', this.id)
				}
			},
			onTimesUp() {
				clearInterval(this.timerInterval)
				this.hidden = true
		    },

		    startTimer() {
		    	this.timerInterval = setInterval(() => (this.timePassed += 1), 1000);
		    }
		},
		mounted() {
			if (this.timing > 0) {
				this.startTimer()
			}
		}
	}
</script>