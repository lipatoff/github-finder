<template>
	<div class="user">
	  <img class="user__img" :src="user.avatar_url" alt="User">
	  <div class="user__info">
	    <p class="title">{{ user.name }}</p>
	    <p>Repos count: {{ user.repos_length }}</p>
	  </div>
	</div>
</template>

<script>
export default {
	props: {
		user: {
			type: Object,
			required: true
		},
	}
}
</script>

<style lang="scss" scoped>
.user{
	display: flex;
	align-items: flex-start;
	margin-top: 30px;
	width: 400px;
	&__img{
		width: 100px;
		border-radius: 50%;
		flex-shrink: 0;
	}
	&__info{
		padding-left: 20px;
	}
}
</style>