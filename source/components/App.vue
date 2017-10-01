<template>
	<div id='app'>

		<h1>When would you like your fresh bread?</h1>

			<div class="day-time-picker">
				<div class="day-picker">
					<img class="icon" src="../img/chevron-left.png">
					<div class="small-caps"> {{ endDay }} </div>
					<img class="icon" src="../img/chevron-right.png">
				</div>
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
			</div>

		<div class="result">
			<div class="label small-caps">Start at</div>
			<div class="medium-text">{{ startDay }} , {{ startTime }}</div>
		</div>
		<button class="text-btn">View plan</button>
		<button v-on:click="getEndTime">Start Plan</button>
	</div>
</template>

<script>

import data from 'js/data'
import moment from 'moment'
import 'vue-awesome/icons'

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
			steps: data.steps,
			endDay: "Saturday",
			startDay: "Friday",
			startTime: "10:40",
			dm: 5
		}
	},
	methods: {
		moment: function() {
			return moment(dm,"MM/DD/YYYY").format('YYYY-MM-DD');
		},
		getEndTime(){
			// this shoiuld return the sliders turned into moment.js time	
			console.log(this.$refs.hour.swiper.realIndex + " : " + this.swiperSlidesMinutes[this.$refs.minutes.swiper.realIndex] );
			console.log("step: " + this.steps[2].name + " duration : " + this.steps[2].duration)
		},
		getPlanDuration(){
			// returns planDuration in minutes
			var planDuration = 0
			for (var step in this.steps){
				planDuration = planDuration + this.steps[step].duration
			}
			return planDuration;
		}
	},
	computed: {
		// take the selected time, day , hour and minutes

		// substract the duration of the plan

		// take the new time and put it into a format
	
   
    },
	mounted() {
		
	 	console.log('THIS MOMENT: ' + moment(moment(),"MM/DD/YYYY").format('DD-MM-YYYY'));
	 
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
		width:7em;
		height:100px;
		line-height: 100px;
		margin: 5%;
		background-color: #EEE;
	}
	h1{
		font-size: 2em;
		text-align: center;
	}
	.icon{
		height: 16px;
		margin: 2px 10px 0 10px;
	}
	.day-time-picker{
		display: flex;
		flex-direction: column;
		text-align: center;
	}
	.day-picker{
		display: flex;
		flex-direction: row;
		justify-content: center;
	}
	.time-picker{
		display: flex;
		flex-direction: row;
	}
	.small-caps{
		text-transform: uppercase;
		font-weight: 500;
		letter-spacing: 1px;
	}
	.colon{
		width: 20px;
	}
	.numbers-large{
		font-size: 6em;
		text-align: center;
	}
	.medium-text{
		font-size: 1.25em;
	}
	.result{
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
	.text-btn{
		border: none;
		text-decoration: underline;
		color: blue;
	}
}
</style>
