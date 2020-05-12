<template>
	<div class="theme-container">
		<Navbar />
		<main class="page">
			<div class="theme-custom-content content__default">
				<slot name="top" />

				<Content class="theme-custom-content" />

				<Content slot-key="center" />

				<AlgoliaSearchBox :options="algolia" />

				<div v-if="data.help && data.help.length" class="row help">
					<div
						v-for="(helpItem, index) in data.help"
						:key="index"
						class="column helpItem"
					>
						<a
							v-if="helpItem.link"
							:href="helpItem.link"
							tabindex="1"
						>
							<div class="card">
								<header v-if="helpItem.faqApp">
									<CellphoneAndroidIcon />
									<h3>{{ helpItem.title }}</h3>
								</header>
								<header v-else-if="helpItem.faqExt">
									<PuzzleIcon />
									<h3>{{ helpItem.title }}</h3>
								</header>
								<header v-else-if="helpItem.guides">
									<ClipboardListIcon />
									<h3>{{ helpItem.title }}</h3>
								</header>
								<header v-else-if="helpItem.forks">
									<SourceForkIcon />
									<h3>{{ helpItem.title }}</h3>
								</header>
								<header v-else-if="helpItem.contribution">
									<LifebuoyIcon />
									<h3>{{ helpItem.title }}</h3>
								</header>
								<header v-else-if="helpItem.icon">
									<MaterialIcon
										:icon-name="helpItem.icon"
										icon-only
									/>
									<h3>{{ helpItem.title }}</h3>
								</header>
								<p>{{ helpItem.description }}</p>
							</div>
						</a>
						<a
							v-else-if="helpItem.linkExt"
							:href="helpItem.linkExt"
							target="_blank"
							rel="noreferrer"
							tabindex="1"
						>
							<div class="card">
								<header v-if="helpItem.discord">
									<DiscordIcon />
									<h3>{{ helpItem.title }}</h3>
								</header>
								<header v-else-if="helpItem.reddit">
									<RedditIcon />
									<h3>{{ helpItem.title }}</h3>
								</header>
								<header v-else-if="helpItem.github">
									<GithubIcon />
									<h3>{{ helpItem.title }}</h3>
								</header>
								<header v-else-if="helpItem.icon">
									<MaterialIcon
										:icon-name="helpItem.icon"
										icon-only
									/>
									<h3>{{ helpItem.title }}</h3>
								</header>
								<p>{{ helpItem.description }}</p>
							</div>
						</a>
					</div>
				</div>

				<slot name="bottom" />
			</div>
		</main>
	</div>
</template>

<script>
import Navbar from "@theme/components/Navbar.vue";
import AlgoliaSearchBox from "../theme/components/AlgoliaSearchBox.vue";

import CellphoneAndroidIcon from "vue-material-design-icons/CellphoneAndroid.vue";
import ClipboardListIcon from "vue-material-design-icons/ClipboardList.vue";
import SourceForkIcon from "vue-material-design-icons/SourceFork.vue";
import PuzzleIcon from "vue-material-design-icons/Puzzle.vue";
import DiscordIcon from "vue-material-design-icons/Discord.vue";
import RedditIcon from "vue-material-design-icons/Reddit.vue";
import GithubIcon from "vue-material-design-icons/Github.vue";
import LifebuoyIcon from "vue-material-design-icons/Lifebuoy.vue";

export default {
	components: {
		Navbar,
		AlgoliaSearchBox,
		CellphoneAndroidIcon,
		ClipboardListIcon,
		SourceForkIcon,
		PuzzleIcon,
		DiscordIcon,
		RedditIcon,
		GithubIcon,
		LifebuoyIcon
	},

	computed: {
		data() {
			return this.$page.frontmatter;
		},

		algolia() {
			return (
				this.$themeLocaleConfig.algolia ||
				this.$site.themeConfig.algolia ||
				{}
			);
		},

		isAlgoliaSearch() {
			return (
				this.algolia && this.algolia.apiKey && this.algolia.indexName
			);
		}
	}
};
</script>

<style lang="stylus">
.page
	padding-left 0 !important
	padding-bottom 2rem
	display block

.theme-custom-content
	max-width 75rem
	margin 0 auto
	padding 2rem 2.5rem
	*
		box-sizing border-box
	h1
		text-align center
		.header-anchor
			display none
	h3
		.header-anchor
			display none
	.content__center
		text-align center
		margin-bottom 2rem
	.algolia-search-wrapper
		width 100%
		text-align center
		margin-bottom 5rem
		.algolia-autocomplete
			width 50%
			input
				background #ffffff url('/assets/img/search.83621669.svg') 0.6rem 0.9rem no-repeat
				background-size 1rem
				box-shadow 0 0 30px rgba(177,174,174,0.322)
				border 1px solid #cfd4db
				font-size 1.2rem
				height 3rem
				width 100%
	.card
		background-color white
		border 1px solid #cfd4db
		border-radius 6px
		box-shadow 0 0 30px #b1aeae52
		color $accentColor
		height 12rem
		overflow hidden
		padding 0.5rem
		text-align center
		user-select none
		width auto
		header
			margin-top 1.25rem
			white-space nowrap
			.material-icons,
			.material-design-icon
				font-size 2.5em
				color $accentColorSecondary
			.material-design-icon > .material-design-icon__svg
				position relative
			h3
				margin 10px
		p
			color #566573
			font-weight 400
			font-size 0.95rem
		&:hover
			position relative
			top -5px
	.column
		float left
		padding 0.5rem
		width 25%
		a:focus
			box-shadow none
			outline none
			.card
				box-shadow 0 0 30px #b1aeae52, 0 0 0 1px #fff, 0 0 0 3px rgba(50, 100, 150, 0.4)
				outline none
	.row
		margin 0 -5px
		&:after
			content ''
			display table
			clear both

@media screen and (max-width $MQMobile)
	.theme-custom-content
		padding 2rem 0.75rem
		padding-bottom 0
		h1
			margin-bottom 0.5rem
		.content__center
			margin-top 0
			padding-top 0
		.algolia-search-wrapper
			width 100%
			margin-bottom 1rem
			.algolia-autocomplete
				width 100%
				padding 0.4rem 0.65rem
				input
					width 100%
					left 0
		.column
			width 100%
			display block
			margin-bottom 0
			margin-top 0
			padding 0.4rem 1rem
		.card
			height auto
			width auto
			header
				margin-top 1rem
				.material-icons,
				.material-design-icon
					font-size 1.6em
				h3
					font-size 1.5rem
					display inline-block
					margin 0
			p
				font-size 1rem
			&:hover
				position inherit
				top unset
</style>