<template>
    <div v-bind:class="{ grid: isActive }"></div>
</template>
<script>
    export default {
        name: 'Grid',
        data: function(){
            return {
              isActive: 1
            }
        }
    }
</script>
<style>
.grid{
    width: 5px;
    height: 5px;
    display: inline-block;
    border: 1px solid #eee;
    background-color: #555;
}
</style>