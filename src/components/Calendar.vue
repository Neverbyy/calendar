<script setup>

const props = defineProps({
	date: {
		type: String,
		default: '',
	},
})

const emit = defineEmits(['select'])

</script>

<template>
	<div class="calendar">

    </div>
</template>

<style scoped lang="scss">
.calendar {
	width: 100%;

}
</style>


