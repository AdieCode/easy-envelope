<template>
    <div class="button-container">
        Sign out
    </div>
</template>

<script setup>

// const props = defineProps({
// })
</script>

<style scoped>
.button-container{
    background-color: #574E76;
    min-width: 160px;
    height: 60px;
    position: absolute;
    top: 20px;
    right: 20px;
    border-radius: 20px;
    border:  1px solid #FFC857;
    font-size: 24px;
    color: #FFC857;
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