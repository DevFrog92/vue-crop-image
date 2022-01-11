<template>
	<div id="app">
		<crop-image />
	</div>
</template>

<script>
import CropImage from './components/CropImage.vue';
export default {
	components: { CropImage },
};
</script>

<style lang="scss">
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
}
</style>
