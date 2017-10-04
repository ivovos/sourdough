<template>
	<div id='app'>

		<h1>When do you want fresh bread?</h1>

			<div class="day-time-picker">
				<div class="day-picker">
					<img @click="substractDay" class="icon" src="../img/chevron-left.png">
					<div class="small-caps"> {{ daysOfTheWeek[endDay] }} </div>
					<img @click="addDay" class="icon" src="../img/chevron-right.png">
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
			<div class="medium-text">{{ daysOfTheWeek[startDay] }} , {{ startHour }}:{{ startMinutes }} </div>
		</div>
		<button @click="showPlan">Create Plan</button>

		<div v-if="planVisible" class="plan-view">
			<ul>
				<button @click="showPlan" class="text-btn">
				Back</button>

				<li v-for="step in steps" :key="step.id">
					{{ step.stepTime }} {{ step.name }} {{ step.duration  }} min 
				</li>
			</ul>
		</div>

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
			stepTimes: ["08:00","10:00","12:00"],
			daysOfTheWeek: ["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"],
			startTime: null,
			endDay: 5,
			startDay: 5,
			startHour: 10,
			startMinutes: 40,
			planVisible: false
		}
	},
	methods: {
		moment: function() {
			return moment(dm,"MM/DD/YYYY").format('YYYY-MM-DD');
		},
		substractDay: function(){
			this.endDay = this.endDay - 1;
			if(this.endDay < 0){
				this.endDay = 6;
			}
		},
		addDay: function(){
			this.endDay = this.endDay + 1;
				if(this.endDay > 6){
				this.endDay = 0;
			}
		},
		getEndTime(){
			var endHour = this.$refs.hour.swiper.realIndex;
			var endMinutes = this.swiperSlidesMinutes[this.$refs.minutes.swiper.realIndex];
			var endTime = moment().add(1, 'days').set({'hour': endHour, 'minute':endMinutes}); 
			return endTime;
		},
		getPlanDuration(){
			// returns planDuration in minutes
			var planDuration = 0
			for (var step in this.steps){
				planDuration = planDuration + this.steps[step].duration
			}
			return planDuration;
		},
		createPlan(){
			console.log(this.getPlanDuration());
			var stepTimes = []
			console.log('end time ='+this.getEndTime().format('dddd hh mm'));
			this.startTime = this.getEndTime().subtract(this.getPlanDuration(),'minute');
			console.log('starttime ='+this.startTime.format('dddd hh mm'));


			// for (var i = 0; i < thissteps; i++) {
   //  		alert(myStringArray[i]);
   //  		//Do something
			// }

			console.log('startTime: '+this.startTime);
			for (var i = 0; i < this.steps.length; i++) {
				this.steps[i].stepTime = this.startTime.add(this.steps[i].duration).format('ddd hh mm');
			} // this doesn't work because the moment.js object is mutuable?

			// console.log(this.steps.length);
			// console.log(this.steps[2].stepTime);

			//this.steps[1].stepTime = this.startTime.add(this.steps[0].duration, 'minutes').format('ddd hh mm');
			// strartTime = getEndtime - planDuration
			// stepTime[0] = startTime + 
			// stepTime[1] = starTime + steps[0].duration
			// stepTime[2] = starTime + steps[0].duration + steps[1].duration

			// console.log('start'= startTime + ' , step 1:' + stepTime[0] + ' ,' + stepTime[1])
			},
		showPlan(){
			this.createPlan();
			this.planVisible = !this.planVisible
		}
	},
	computed: {
		defineEndTime(){
			this.endTime = moment().format('dddd');
		}   
    },
	mounted() {
		var lastHour = 10; // when is the latest you want to start making bread
		var dateNow = moment();
		if(dateNow.hour() >= lastHour){
			dateNow.add(1,'day').set({'hour':9,'minute':0});
		}

		console.log('>> D A T E ' + dateNow.format("dddd, h:mm"));
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
		justify-content: space-between;
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
	.plan-view{
		display: flex;
		z-index: 5;
		position: absolute;
		background-color: white;
		width: 100%;
		height: 100%;		

	}
}
</style>
