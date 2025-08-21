<script setup>
import { ref } from 'vue'
import Calendar from './components/Calendar.vue'

const selectedDate = ref('')
const locale = ref('ru')

const handleSelect = (date) => {
	selectedDate.value = date
}

const handleLocaleChange = (newLocale) => {
	locale.value = newLocale
}
</script>

<template>
	<div class="page">
		<div class="page__container">
			<div class="page__header">
				<h1 class="page__title">Календарь</h1>
				<div class="page__locale">
					<button
						class="page__localeBtn"
						:class="{ 'page__localeBtn--active': locale === 'ru' }"
						@click="handleLocaleChange('ru')"
					>
						RU
					</button>
					<button
						class="page__localeBtn"
						:class="{ 'page__localeBtn--active': locale === 'en' }"
						@click="handleLocaleChange('en')"
					>
						EN
					</button>
				</div>
			</div>
			<Calendar :locale="locale" @select="handleSelect" />
			<div class="page__selected">
				<p class="page__selectedText">Выбранная дата: <span class="page__selectedValue">{{ selectedDate }}</span></p>
			</div>
		</div>
	</div>
</template>

<style scoped lang="scss">
.page {
	min-height: 100vh;
	width: 100%;
	display: flex;
	align-items: flex-start;
	justify-content: center;
	padding: 24px;

	&__container { 
		width: 100%; 
		max-width: 640px; 
	}

	&__header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-bottom: 16px;
	}

	&__title { 
		font-size: 24px; 
		line-height: 32px; 
		font-weight: 700; 
		color: #000;
	}

	&__locale {
		display: flex;
		gap: 4px;
	}

	&__localeBtn {
		padding: 6px 12px;
		border: 1px solid #e5e7eb;
		border-radius: 6px;
		background: transparent;
		cursor: pointer;
		font-size: 14px;
		font-weight: 500;
		transition: all .15s ease-in-out;

		&:hover {
			border-color: #6366f1;
		}

		&--active {
			background-color: #6366f1;
			color: white;
			border-color: #6366f1;
		}
	}

	&__selected { 
		margin-top: 16px; 
	}

	&__selectedText { 
		font-size: 14px; 
		line-height: 20px; 
		color: #000;
	}

	&__selectedValue { 
		font-weight: 600; 
		color: #000; 
	}
}
</style>


