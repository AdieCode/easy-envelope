<template>
    <div class="confirmation-container">
        <div>

            <div class="header">
                <h1>Confirm</h1>
                <h4>(I want to delete this)</h4>
            </div>
    
            <div style="display: flex;">
                <my-button name="Yes" :width="120" :height="70" :textSize="30" :round="10" margin="10px 10px" @click="() => buttonClick(true)"/>
                <my-button name="No" :width="120" :height="70" :textSize="30" :round="10" margin="10px 10px" @click="() => buttonClick(false)"/>
            </div>
        </div>
        </div>
    <div class="blur-back"></div>
</template>

<script setup>

const props = defineProps({
    funcHandeler: Function,
});

function buttonClick(value){
    props.funcHandeler(value);
}

</script>

<style scoped>
.confirmation-container{
    width: 520px;
    height: 400px;
    border-radius: 10px;
    border: 4px solid #FFC857;
    background-color: #68617D;
    position: fixed;
    z-index: 3;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    align-content: center;
    animation: bounce 0.2s;
    animation-fill-mode: both;
}

.header{
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    align-items: center;
    margin-bottom: 40px;
    gap: 4px;
}

h1{
    font-size: 45px;
    font-weight: 600;
    color: #fff;
}

h4{
    font-size: 20px;
    font-weight: 400;
    color: #fff;
}

.blur-back{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 2;
    backdrop-filter: blur(5px); /* Apply background blur effect */
    background-color: #574e7676; /* Background color with opacity for the blur effect */
    animation: show 0.2s;
}

@keyframes bounce {
    0%{
        top: 52%;
        opacity: 0;
    }
    100%{
        top: 50%;
        opacity: 100%;

    }
}

@keyframes show {
    0%{
        opacity: 0;
    }
    100%{
        opacity: 100%;

    }
}


</style>