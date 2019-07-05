<template>
	<div class="repos__wrapper">
	<!-- items header -->
        <div class="repos-info">
            <p class="repos-title" :class="{active: currentSort === 'name'}" @click="sort('name')">Name &darr;</p>
            <p class="repos-title" :class="{active: currentSort === 'stargazers_count'}" @click="sort('stargazers_count')">Stars &darr;</p>
        </div>

        <!-- item -->
        <div class="repos-item" v-for="repo in reposSort" :key="repo.id">
          <div class="repos-info">
            <a class="link" target="_blank" :href="repo.html_url">{{ repo.name }}</a>
            <span> {{ repo.stargazers_count }} ⭐</span>
          </div>
        </div>

        <!-- buttons -->
        <div class="button-list" v-if="page.count > 1">
          <button class="btn" :class="{btnPrimary: page.current > 1}" @click="prevPage"> &larr; </button>
          <button class="btn" :class="{btnPrimary: page.current < page.count}" @click="nextPage"> &rarr; </button>
          <span class="count">{{ page.current }} / {{ page.count }}</span>
        </div>
	</div>
</template>

<script>
export default {
	props: {
		repos: {
			type: Array,
			required: true
		}
	},
	data() {
		return {
			currentSort: 'name',
			currentSortDir: 'asc',
			page: {
				current: 1,
				length: 5,
				count: 1
			}
		}
	},
	created () {
		this.page.count = Math.ceil(this.repos.length / this.page.length)
	},
  	computed: {
    	reposSort() {
    		return this.repos.sort((a,b) => {
    			if (!this.currentSort) return false
    			let mod = 1
    			if (this.currentSortDir === 'desc') mod = -1
    			if (a[this.currentSort] < b[this.currentSort]) return -1 * mod
    			if (a[this.currentSort] > b[this.currentSort]) return 1 * mod
    		}).filter((row, index) => {
    			let start = (this.page.current-1)*this.page.length
    			let end = this.page.current * this.page.length
    			if ( (index >= start) && (index < end) ) {
    				return true
    			}
    		})
    	}
	},
	methods: {
		sort(e) {
			if (e === this.currentSort) {
				this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc'
			} else {
				this.currentSort = e
			}
		},
		//Pagination
		prevPage() {
			if (this.page.current > 1) this.page.current-=1
		},
		nextPage() {
			if ( (this.page.current * this.page.length) < this.repos.length) this.page.current+=1
		}
	}
}
</script>

<style lang="scss" scoped>
.repos__wrapper{
	width: 400px;
	margin: 30px 0;
}
.repos-info{
	display: flex;
	align-items: center;
	justify-content: space-between;
	margin-bottom: 10px;
	padding: 10px 0;
	border-bottom: 1px solid #dbdbdb;
}
.repos-title{
	cursor: pointer;
	color: #ccc;
	&.active{
		color: #000;
	}
}
.button-list{
	display: flex;
	align-items: center;
	.count{
		margin-left: auto;
		color: #ccc;
	}
}
</style>
