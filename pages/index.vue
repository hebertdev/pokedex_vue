<template>
	<div>
		<div class="banner-information"  :style="{ backgroundImage:`url(${backgroundUrl})`}">
		</div>
	</div>
</template>

<script>
	import backgroundUrl from '~/static/img/banner.png'


	export default {

		components: {

		},
		data(){
			return{
				backgroundUrl
			}
		}
	}
</script>

<style>



.banner-information{
  width: 100%;
  height: 100vh;
  background-repeat: no-repeat;
  background-size: cover;
  position: relative;
  background-position: center;

}

.banner-information::before{
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
 
}

</style>
