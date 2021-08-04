<template>
	<div class="calendar">
		<table class="table">
			<thead>
				<tr>
					<td>
						<button class="arrow__btn" @click="decresase">&#128896;</button>
					</td>
					<td colspan="5"> {{monthes[month]}} {{year}}</td>
					<td>
						<button class="arrow__btn" @click="increase">&#128898;</button>
					</td>
				</tr>
				<tr>
					<td v-for="d in day" :key="d">{{d}}</td>
				</tr>
			</thead>
			<tbody>
				<tr v-for="week in calendar" :key="week.index">
					<td 
						v-for="(day, index) in week" 
						:key="index" :class="day.mark" 
						class="day-cell"
						@click="changeDay(day.index)"
						>{{ day.index }}
						
					</td>
				</tr>
			</tbody>
		</table>
	 	<button class="change-lang__btn" @click="changeLang"> {{btnTitle}} </button>
	</div>
</template>

<script>
export default {
	name: 'Calendar',
	data() {
		return {
			month: new Date().getMonth(),
			year: new Date().getFullYear(),
			dFirstMonth: 0,
			day: ["Sun", "Mon", "Tue","Wed","Thu","Fri","Sat"],
			monthes: ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"],
			date: new Date(),
			currentDay: new Date().getDate(),
			isEng: true,
			btnTitle: 'Change language'
		}
	},
	methods: {
		changeDay(day) {
			this.date.setDate(day);
			this.currentDay = this.date.getDate();
		},
		decresase() {
			this.month--;
			if (this.month < 0) {
				this.month = 12;
				this.month--;
				this.year--;
			}
		},
		increase() {
			this.month++;
			if (this.month > 11) {
				this.month = -1;
				this.month++;
				this.year++;
			}
		},
		changeLang() {
			this.isEng = !this.isEng;
			this.language
		}
	},
	computed: {
		calendar() {
			let days = [];
			let day;
			let week = 0;

			days[week] = [];

			let dLast = new Date(this.year, this.month + 1, 0).getDate();
			for (let i = 1; i <=dLast; i++) {
				if (new Date(this.year, this.month, i).getDay() != this.dFirstMonth) {
					day = {index: i}
					if (day.index === this.currentDay) {
						day.mark = 'today';
					}
					days[week].push(day);
				} 
				else {
					week++;
					days[week] = [];
					day = {index: i};
					days[week].push(day);
				}
			}
			if (days[0].length > 0) {
				for( let i = days[0].length; i < 7; i++) {
					days[0].unshift('');
				}
			}
		
			return days
		},
		language() {
			if (this.isEng) {
				this.day = ["Sun", "Mon", "Tue","Wed","Thu","Fri","Sat"];
				this.monthes = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"];
				this.btnTitle = "Change language";
			} 
			else {
				this.day = ["Вс", "Пн", "Вт","Ср","Чт","Пт","Сб"];
				this.monthes = ["Янв", "Фев", "Мар", "Апр", "Май", "Июн", "Июл", "Авг", "Сен", "Окт", "Ноя", "Дек"];
				this.btnTitle = "Сменить язык";
			}
		}
	}
}
</script>


<style>

	* {
		box-sizing: border-box;
		font-size: 10px;
		font-family: sans-serif;
	}

	.calendar {
		display: flex;
	}

	td {
		text-align: center;
		width: 25px;
		height: 25px;
	}

	.table {
		border: 1px solid #ccc;
		height: 200px;
		width: 200px;
	}

	.today {
		border: 1px solid #61C5E2;
	}

	.day-cell, button {
		cursor: pointer;
	}

	.day-cell {
		color: gray;
	}

	.arrow__btn {
		border: none;
		background: #fff;
	}	

	.change-lang__btn {
		height: 25px;
		padding: 5px;
		margin-left: 10px;
		width: 100px;
	}
</style>
