<template>
	<div class="w-full pt-10 md:pt-20 pr-12 pb-5 pl-12">
		<TheDiscussion />
		<TheDiscussionPosts />
		<!-- New Post -->
		<div class="mt-8">
			<template v-if="discussion.isClosed">
				<Alert icon="info">This discussion is closed. It cannot accept replies anymore.</Alert>
			</template>
			<template v-else-if="$auth.loggedIn">
				<NewPost />
			</template>
			<template v-else>
				<p class="text-sm text-gray-600">
					Please 
					<nuxt-link to="/auth/signin" class="text-blue-400 font-medium text-decoration">sign in</nuxt-link> or 
					<nuxt-link to="/auth/signup" class="text-blue-400 font-medium">sign up</nuxt-link> to participate in this discussion!
				</p>
			</template>
		</div>
	</div>
</template>

<script>
	import Alert from '~/components/Alert'
	import TheDiscussion from '~/components/Discussions/TheDiscussion'
	import NewPost from '~/components/Posts/NewPost'
	import TheDiscussionPosts from '~/components/Posts/TheDiscussionPosts'
	import { mapGetters } from 'vuex'
	export default {
		components: {
			TheDiscussion,
			TheDiscussionPosts,
			NewPost,
			Alert,
		},
		computed: {
			...mapGetters({
				discussion: 'currentDiscussion'
			})
		},
		head() {
			return {
				title: `${this.discussion.title} - Forum`
			}
		}
	}
</script>