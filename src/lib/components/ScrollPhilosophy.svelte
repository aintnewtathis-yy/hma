<script>
	import { gsap } from 'gsap/dist/gsap.js';
	import { ScrollTrigger } from 'gsap/dist/ScrollTrigger';
	import { onDestroy, onMount } from 'svelte';
	import { CMS_URL } from '$lib/globals.js';

	let { data } = $props()
	let images = data.images.data

	gsap.registerPlugin(ScrollTrigger);

	let tl;

	const scrollAnimation = (scrollDivs) => {
		tl = gsap.timeline({
			scrollTrigger: {
				trigger: '.scroll-philosophy-section',
				pin: true,
				start: 'top 10px',
				end: `+=${scrollDivs[0].scrollHeight * 3}px`,
				scrub: 1
			}
		});

		tl.to(scrollDivs[3], {
			height: '0%'
		})
			.to(scrollDivs[2], {
				height: '0%'
			})
			.to(scrollDivs[1], {
				height: '0%'
			});
	};

	onMount(() => {

		const scrollDivs = document.querySelectorAll('.scroll-philosophy div');

		if (window.innerWidth > 1024) {
			setTimeout(() => {
				scrollAnimation(scrollDivs);
			}, 50);
		}
	});

	onDestroy(() => {
		if (tl) {
			tl.kill();
		}
	});
</script>

<section class="scroll-philosophy-section --margin-bottom">
	<div class="container">
		<div class="scroll-philosophy">
			{#each images as image}
				<div style="background-image: url('{CMS_URL + image.attributes.url}');"></div>
			{/each}
			<h3>{data.text}</h3>
		</div>
	</div>
</section>

<style lang="scss">
	.scroll-philosophy {
		max-height: 1920px;
		height: calc(100vh - 20px);
		display: flex;
		align-items: end;

		&-section {
			max-height: 1920px;
			height: calc(100vh - 20px);
			border-radius: var(--border-radius);
			overflow: hidden;
		}

		div {
			top: 0;
			left: 0;
			height: 100%;
			width: 100%;
			position: absolute;
			background-attachment: fixed;
			background-size: cover;
			background-position: center top;
			background-repeat: no-repeat;
			filter: brightness(60%);
			border-radius: var(--border-radius);
			overflow: hidden;

			@media (max-width: 1400px) {
				background-size: 130%;
			}
			@media (max-width: 1024px) {
				background-attachment: scroll;
				background-size: cover;
			}
		}
		h3 {
			position: absolute;
			top: 50%;
			left: 50%;
			transform: translate(-50%, -50%);
			text-align: center;
			width: 80%;
			text-wrap: balance;
			color: #fff;
			line-height: 100%;
			font-size: clamp(50px, calc(75 / 1600 * 100vw), 75px);

			@media (max-width: 767px) {
				font-size: clamp(30px, calc(60 / 1600 * 100vw), 60px);
			}
		}
	}
</style>
