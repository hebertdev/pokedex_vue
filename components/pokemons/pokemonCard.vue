<template>
 <div >
   <div class="card">
     
     {{pokes.name}}
     
   </div>
 </div>
</template>
<script>
export default {
 props: {
   pokes: {
     type: Object
   }
 },
}
</script>
<style>




</style>