<template>
	<GunTable />
</template>

<script>
import GunTable from './components/GunTable.vue'

export default {
	name: 'App',
	components: {
		GunTable
	}
}
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	/* text-align: center; */
	color: #2c3e50;
	margin-top: 20px;
}
</style>
