<script setup>

const now = new Date()

const today = now.toLocaleDateString('ru-RU', {})
</script>

<template>
	<div class="page">
		<div class="page__container">
			<h1 class="page__title">Календарь</h1>
			<div class="page__selected">
				<p class="page__selectedText">Выбранная дата: <span class="page__selectedValue">{{ today }}</span></p>
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

	&__title { 
		font-size: 24px; 
		line-height: 32px; 
		font-weight: 700; 
		margin-bottom: 16px;
		color: #000;
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


