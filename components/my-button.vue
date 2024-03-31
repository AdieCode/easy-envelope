<template>
    <div class="button-container" :style="{ width: `${width}px`, height: `${height}px`, fontSize:  `${textSize}px`, borderRadius: `${round}px`, margin: margin}">
        {{ name }}
    </div>
</template>

<script setup>

const props = defineProps({
  name: {type: String, default: 'Button'},
  width: {type: Number, default: 160},
  height: {type: Number, default: 80},
  textSize: {type: Number, default: 32},
  round: {type: Number, default: 20},
  margin: {type: String, default: '20px 20px'}
});
</script>

<style scoped>
.button-container{
    background-color: #FFC857;
    min-width: 160px;
    min-height: 40px;
    width: 190px;
    height: 90px;
    border-radius: 20px;
    margin: 20px;
    box-shadow: 0px 0px #1B3F5A;
    color: #1B3F5A;
    font-weight: 800;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    transition: 0.2s;
}

.button-container:hover{
    transform: translate(-6px, -6px);
    
    box-shadow: 6px 6px #1B3F5A;
}


</style>