<template>
  <div class="popup">
		<div class="popup-inner">
			<slot></slot>
			<button class="popup-close" @click="TogglePopup()">
				
			</button>
		</div>
	</div>
</template>

<script>
export default {
	props: ['TogglePopup']
}
</script>

<style>

</style>