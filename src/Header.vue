<template>
	<div>
		<h1>Let's Tango!</h1>
		<ul class="nav nav-pills">
			<router-link class="nav-link" to="/" tag="li" active-class="active" exact>Home</router-link>
			<router-link class="nav-link" to="/add" tag="li" active-class="active">Add word</router-link>
		</ul>
	</div>
</template>

<script>
	export default {
	}
</script>

<style>
</style>