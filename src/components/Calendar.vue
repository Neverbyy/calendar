<script setup>
import { ref } from 'vue'

const props = defineProps({
	date: {
		type: String,
		default: '',
	},
})

const emit = defineEmits(['select'])

const today = new Date()
const currentMonth = ref(today.getMonth())
const currentYear = ref(today.getFullYear())

const monthNames = [
	'Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь',
	'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь',
]

const weekDayNames = ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс']

const handlePrevMonth = () => {
	if (currentMonth.value === 0) {
		currentMonth.value = 11
		currentYear.value -= 1
		return
	}
	currentMonth.value -= 1
}

const handleNextMonth = () => {
	if (currentMonth.value === 11) {
		currentMonth.value = 0
		currentYear.value += 1
		return
	}
	currentMonth.value += 1
}

</script>

<template>
	<div class="calendar">
		<div class="calendar__nav">
			<button
				class="calendar__navBtn"
				@click="handlePrevMonth"
			>
				‹
			</button>
			<div class="calendar__title">Temp title</div>
			<button
				class="calendar__navBtn"
				@click="handleNextMonth"
			>
				›
			</button>
		</div>

	</div>
</template>

<style scoped lang="scss">
.calendar {
	width: 100%;

	&__nav {
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-bottom: 8px;
	}

	&__navBtn {
		padding: 4px 8px;
		border: 1px solid #e5e7eb;
		border-radius: 6px;
		cursor: pointer;
		transition: border-color .15s ease-in-out, background-color .15s ease-in-out;

		&:hover { 
            border-color: #6366f1; 
        }
	}

	&__title {
		font-size: 18px;
		font-weight: 600;
	}

	
}
</style>


