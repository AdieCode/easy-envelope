<template>
    <div class="editing-container">
        <div class="header">
            <h1>{{ propertys.type }} envelope</h1>
            <h4>click outside to go back</h4>
        </div>
        
        <form action="">
            <div class="edits">
                <div>
                    <label for="" >Category</label>
                    <input type="text" placeholder="Enter a category" v-model="categoryInput">
                </div>
    
                <div>
                    <label for="" >Budget</label>
                    <input type="number" placeholder="Enter a number" :min="0" v-model="budgetInput">
                </div>
            </div>
        </form>

        <my-button :name="propertys.type" :width="184" :height="60" :textSize="24" :round="10" margin="10px 10px" @click="buttonClick"/>
    </div>
    <div class="blur-back" @click="backGroundClick"></div>
</template>


<script setup>
import { ref } from 'vue';

let categoryInput = ref('');
let budgetInput = ref('');

const props = defineProps({
    stack_id: String,
    toggleVisibility:  Function,
    propertys: Object
});

function buttonClick() {
    props.propertys.handler(props.propertys.id ,categoryInput.value, budgetInput.value);
    props.toggleVisibility()
}

function backGroundClick(){
    props.toggleVisibility()
}

onMounted(() => {
    categoryInput.value = props.propertys.firstEdit;
    budgetInput.value = props.propertys.secondEdit;
})
</script>

<style scoped>

.editing-container{
    width: 520px;
    height: 550px;
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
    justify-content: space-evenly;
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
    gap: 4px;
}

h1{
    font-size: 45px;
    font-weight: 600;
    color: #fff;
}

h4{
    font-weight: 400;
    color: #fff;
}

label{
    display: block;
    padding: 10px;
    font-size: 20px;
    font-weight: 600;
    color: #ffc757;
}

input{
    font-family: "Newsreader", serif;
    width: 290px;
    height: 20px;
    padding: 20px;
    border-radius: 5px;
    background-color: #4B3F72;
    border: 2px solid #119DA4;
    outline: none;
    font-size: 24px;
    font-weight: 400;
    color: #f0f0f0;
}

.edits{
    display: flex;
    flex-direction: column;
    gap: 20px;
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