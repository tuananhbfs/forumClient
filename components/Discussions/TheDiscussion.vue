<template>
	<div class="flex items-center justify-between pb-1 mb-4 border-b border-solid border-gray-200 flex-col sm:flex-row">
		<div class="w-full">
			<h3 class="m-0 tracking-tight font-medium mb-1 text-lg text-gray-900">{{ discussion.title }}</h3>
		</div>
		<div class="flex items-center justify-between flex-row-reverse my-2 sm:my-0">
			<template v-if="user.canEdit">
				<nuxt-link to="/" class="block bg-blue-400 text-white py-1 px-2 transition-all duration-200 ease-in-out text-xs ml-4 text-center w-24 rounded">Edit this discussion</nuxt-link>
				<!-- <div class="flex items-center ml-4">
					<a href="#" class="w-8 h-8 bg-blue-400 text-white flex items-center justify-center rounded-full mr-2" title="Close the discussion" @click.prevent="close">
						<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24" class="fill-current w-5 h-5"><path class="heroicon-ui" d="M7 10V7a5 5 0 1 1 10 0v3h2a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-8c0-1.1.9-2 2-2h2zm2 0h6V7a3 3 0 0 0-6 0v3zm-4 2v8h14v-8H5zm7 2a1 1 0 0 1 1 1v2a1 1 0 0 1-2 0v-2a1 1 0 0 1 1-1z"/></svg>
					</a>
					<a href="#" class="w-8 h-8 bg-gray-500 text-white flex items-center justify-center rounded-full" title="Delete the discussion" @click.prevent="destroy">
						<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24" class="fill-current w-5 h-5"><path class="heroicon-ui" d="M8 6V4c0-1.1.9-2 2-2h4a2 2 0 0 1 2 2v2h5a1 1 0 0 1 0 2h-1v12a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V8H3a1 1 0 1 1 0-2h5zM6 8v12h12V8H6zm8-2V4h-4v2h4zm-4 4a1 1 0 0 1 1 1v6a1 1 0 0 1-2 0v-6a1 1 0 0 1 1-1zm4 0a1 1 0 0 1 1 1v6a1 1 0 0 1-2 0v-6a1 1 0 0 1 1-1z"/></svg>
					</a>
				</div> -->
			</template>
			<nuxt-link :to="channelLink" class="block bg-gray-500 text-white py-1 px-2 transition-all duration-200 ease-in-out text-xs ml-4 text-center w-24 rounded">{{ channel.name }}</nuxt-link>
			<div class="flex items-center justify-center text-gray-600 ml-4">
	          <div class="mr-1">
	            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24" class="w-4 fill-current"><path class="heroicon-ui" d="M2 15V5c0-1.1.9-2 2-2h16a2 2 0 0 1 2 2v15a1 1 0 0 1-1.7.7L16.58 17H4a2 2 0 0 1-2-2zM20 5H4v10h13a1 1 0 0 1 .7.3l2.3 2.29V5z"/></svg>
	          </div>
	          <span class="text-xs font-bold">{{ discussion.posts_count }}</span>
	        </div>
	        <!-- <div class="flex items-center justify-center text-gray-600 ml-4">
	          <div class="mr-1">
	            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"  class="w-4 fill-current"><path class="heroicon-ui" d="M17.62 10H20a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2H8.5c-1.2 0-2.3-.72-2.74-1.79l-3.5-7-.03-.06A3 3 0 0 1 5 9h5V4c0-1.1.9-2 2-2h1.62l4 8zM16 11.24L12.38 4H12v7H5a1 1 0 0 0-.93 1.36l3.5 7.02a1 1 0 0 0 .93.62H16v-8.76zm2 .76v8h2v-8h-2z"/></svg>
	          </div>
	          <span class="text-xs font-bold">{{ discussion.votes_count }}</span>
	        </div> -->
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				discussion: {},
				channel: {},
				user: {},
			}
		},
		computed: {
		    channelLink() {
				return '/channels/' + this.channel.slug
		    },
		},

		async fetch() {
			try {
				let discussion = await this.$axios.$get(`discussions/${this.$route.params.discussion}`)
				this.discussion = discussion.data
				this.user = this.discussion.user
				this.channel = this.discussion.channels.data
				this.$store.commit('SET_CURRENT_DISCUSSION', this.discussion)
			} catch(e) {
				if (e.response) {
					if (e.response.status === 404) 
						this.$nuxt.error({ statusCode: 404, message: 'Discussion not found!' })
				} else {
					this.$nuxt.error({ statusCode: 500, message: 'Server error!' })
				}
			}
		},

		fetchOnServer: true,

		methods: {
			close() {

			},
			destroy() {

			}
		},
	}
</script>