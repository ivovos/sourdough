<template>
	<div id='app'>
		<h1>What time would you like fresh bread?</h1>
		<div class="day-picker"
		<div class="time-picker">
			<swiper :options="sliderOptions" class='pad' ref="hour">
			

				<swiper-slide v-for="slide in swiperSlidesHours" :key="slide.id">
					<span class="numbers-large">{{ slide.toString()}}</span>
				</swiper-slide>

			</swiper>
			<div class="colon"><span class="numbers-large">:</span></div>
			<swiper :options="sliderOptions" class='pad' ref="minutes">

				<swiper-slide v-for="slide in swiperSlidesMinutes" :key="slide.id">
					<span class="numbers-large">{{ slide.toString()}}</span>
				</swiper-slide>

			</swiper>
		</div>
		<button v-on:click="getValue">Create plan</button>
	</div>
</template>

<script>

import data from 'js/data'

export default {
	name: 'app',
	data() {
		return {
			sliderOptions:{
				direction: "vertical",
				centeredSlides: true,
				slidesPerView: 1,
				loop: true,
				initialSlide: 8
			},
			swiperSlidesHours: [0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24],
			swiperSlidesMinutes: ['00','05',10,15,20,25,30,35,40,45,50,55],
			steps: data.steps
		}
	},
	methods: {
		getValue(){
			console.log(this.$refs.hour.swiper.realIndex + " : " + this.swiperSlidesMinutes[this.$refs.minutes.swiper.realIndex] );
			console.log("step: " + this.steps[2].name + " duration : " + this.steps[2].duration)
		}
	},
	computed: {
   
    },
	mounted() {
		// console.log(data.steps)
	}
}
</script>

<style lang='scss'>
@import '../sass/variables';

#app {
	padding: 20px;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	.pad{
/*		background:#EEE;
*/		width:7em;
		height:120px;
		margin: 5%;
	}
	.time-picker{
		display: flex;
		flex-direction: row;
		text-align: center;
	}
	.colon{
		width: 20px;
	}
	.numbers-large{
		font-size: 96px;
		text-align: center;
	}
	button{
		width: 100%;
		height: 44px;
		border: 1px solid #eee;
		background-color: white;
		font-size: 16px;
		align-self: flex-end;
	}

}
</style>
