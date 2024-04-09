<template>
        <div class="stack-bar"  @click="showStackSelection">

            <!-- current stack -->
            <div class="stack">
                <h2>Monthly</h2>
                <div class="side-drop">
                    <img src="../images/icons8-triangle-96.png" alt="">
                </div>
            </div>

            <!-- stack options -->
            <div class="stacks" v-for="(stack, index) in stacks" :key="index" v-if="backgroundBlur" >
                <h1 @click="clickInStackBar"> 
                    {{stack.title}}
                </h1>
                <div class="options" style="display: flex; flex-direction: row;">
                    <div class="Edit">
                        <img src="../images/icons8-edit-480.png" alt="">
                    </div>
                    <div class="delete"> 
                        <img src="../images/icons8-trash-480.png" alt="">
                    </div>
                </div>
            </div>

        </div>
        <div class="blur-back" v-if="backgroundBlur" @click="hideStackSelection"></div>

</template>

<script setup>
import { ref } from 'vue';

const backgroundBlur = ref(false);
const wasClickInStackBar = ref(false)
// const extraClass = ref('');

const props = defineProps({
    clickAction: Function,
    stacks: Array
});

function showStackSelection(){
    if (wasClickInStackBar.value){
        wasClickInStackBar.value = false;
        return 
    }

    backgroundBlur.value = true;
    handle();
}

function clickInStackBar(){
    wasClickInStackBar.value = true;
    backgroundBlur.value = false;  
}

function hideStackSelection(){
    backgroundBlur.value = false;   
}

const handle = () => {
  // Call the click action passed via props
  props.clickAction();
};
</script>

<style scoped>
.stack{
    display: flex;
    align-content: center;
}
.stacks{
    display: flex;
    justify-content: space-between;
    align-content: center;
    padding: 20px;
    
}

.stacks h1{
    font-size: 36px;
    color: #119DA4;
    font-weight: 600;
    transition: 0.2s;
}

.stacks:hover h1{
    transform: translateX(10px);
    color: #FFC857;
}

.stacks img{
    width: 36px;
    height: 36px;
}

.stack-bar{
    display: flex;
    flex-direction: column;
    align-content: center;
    position: absolute;
    transform: translateX(-50%);
    top: 25px;
    left: 50%;
    width: 450px;
    height:auto;
    background-color: #574e76b5;
    border-radius: 20px;
    border: 1px solid #FFC857;
    cursor: pointer;
    transition: 0.3s;
    z-index: 2;
}

.stack-bar:hover{
    background-color: #574e76b5; /* Change the background color of side-drop when hovering */
}

.side-drop img {
    width: 30px;
    height: 30px;
    margin: 20px;
    transition: transform 0.3s ease; /* Add a transition for smoother animation */
}

.stack-bar:hover .side-drop img {
    transform: translateY(5px); /* Move the image slightly when hovering */
}

.stack-bar h2{
    margin: 20px;
    margin-left: 20px;
    width: 350px;   
    font-size: 36px;
    color: #119DA4;
    display: flex;
    align-items: center; 
}

.side-drop{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100px;
    height: 100%;
    border-top-right-radius: 20px;
    border-bottom-right-radius: 20px;
    
}

.side-drop img{
    width: 30px;
    height: 30px;
}

.blur-back{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    backdrop-filter: blur(5px); /* Apply background blur effect */
    background-color: #574e7676; /* Background color with opacity for the blur effect */
    animation: show 0.2s;
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