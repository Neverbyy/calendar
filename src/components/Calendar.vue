<script setup>
import { ref, computed, onMounted } from 'vue'

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

const Title = computed(() => `${monthNames[currentMonth.value]} ${currentYear.value}`)

const formatDate = (date) => `${date.getFullYear()}-${(date.getMonth() + 1)}-${(date.getDate())}`
const selectedDate = ref(props.date || new Date(today.getFullYear(), today.getMonth(), today.getDate()))

const calendarDays = computed(() => {
	const firstOfMonth = new Date(currentYear.value, currentMonth.value, 1)
	const start = (firstOfMonth.getDay() + 6) % 7 
	const totalCells = 42 // 6 недель по 7 дней

	const days = []

	for (let i = 0; i < totalCells; i++) {
		const date = new Date(currentYear.value, currentMonth.value, i - start + 1)
		days.push({
			key: formatDate(date),
			date,
			label: date.getDate(),
			inCurrentMonth: date.getMonth() === currentMonth.value,
			isToday:
				date.getFullYear() === today.getFullYear() &&
				date.getMonth() === today.getMonth() &&
				date.getDate() === today.getDate(),
            isSelected:
				date.getFullYear() === selectedDate.value.getFullYear() &&
				date.getMonth() === selectedDate.value.getMonth() &&
				date.getDate() === selectedDate.value.getDate(),
		})
	}

	return days
})

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

const handleSelectDay = (day) => {
    selectedDate.value = new Date(day.date)
	emit('select', formatDate(selectedDate.value))
}

onMounted(() => {
	// Отображаем выбранную дату (по умолчанию сегодняшнюю)
	emit('select', formatDate(selectedDate.value))
})
</script>

<template>
	<div class="calendar">
		<div class="calendar__nav">
			<button
				class="calendar__navBtn"
				aria-label="Предыдущий месяц"
				@click="handlePrevMonth"
			>
				‹
			</button>
			<div class="calendar__title"> {{ Title }}</div>
			<button
				class="calendar__navBtn"
				aria-label="Следующий месяц"
				@click="handleNextMonth"
			>
				›
			</button>
		</div>

		<div class="calendar__week">
			<div v-for="d in weekDayNames" :key="d" class="calendar__weekDay">
				{{ d }}
			</div>
		</div>
			<div class="calendar__grid">
				<div
					v-for="day in calendarDays"
					:key="day.key"
					class="calendar__day"
					:class="{
						'calendar__day--muted': !day.inCurrentMonth,
						'calendar__day--selected': day.isSelected,
						'calendar__day--today': day.isToday && !day.isSelected,
					}"
					role="button"
					tabindex="0"
					@click="handleSelectDay(day)"
				>
					<span class="calendar__dayLabel">{{ day.label }}</span>
				</div>
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

	&__week {
		display: grid;
		grid-template-columns: repeat(7, 1fr);
		gap: 4px;
		margin-bottom: 4px;
	}

	&__weekDay {
		text-align: center;
		font-size: 12px;
		font-weight: 600;
		padding: 4px 0;
	}

	&__grid {
		display: grid;
		grid-template-columns: repeat(7, 1fr);
		gap: 4px;
	}

	&__day {
		padding: 8px;
		border-radius: 6px;
		user-select: none;
		outline: none;
		border: 1px solid transparent;
		cursor: pointer;
		background: transparent;
		transition: border-color .15s ease-in-out, background-color .15s ease-in-out, box-shadow .15s ease-in-out;
		&:hover { border-color: #e5e7eb; }

		&--muted { opacity: .5; }
		&--selected { box-shadow: 0 0 0 2px #6366f1 inset; }
		&--today { background-color: #fef3c7; }
	}

	&__dayLabel { display: block; text-align: center; }
}
</style>


