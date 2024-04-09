<template>

    <div class="envelope-container">
        
        <!-- envelope info -->
        <div class="left">
            <h1>{{ category }}</h1>
            <h2>R {{ budget }}</h2>
            <p>{{ description }}</p>
        </div>

        <!-- options for envelope -->
        <div class="right">
            <my-button name="Transfer" :width="184" :height="60" :textSize="20" :round="10" margin="10px 10px"/>
            <my-button name="Update" :width="184" :height="60" :textSize="20" :round="10" margin="10px 10px" @click="updateHandeler"/>
        </div>

    </div>

</template>

<script setup>

const props = defineProps({
    category: String,
    budget: String,
    description: String,
    updateHandeler: Function,
});

</script>

<style scoped>

.envelope-container{
    width: 49% ;
    height: 188px;
    border-radius: 10px;
    background-color: #574E76;
    box-shadow: 5px 5px #1F2041;
    animation: envelope-move 0.2s;  
    display: flex;  
    justify-content: space-between;
    transition: 0.2s;
}


.left{
    width: 50%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    margin-left: 25px;
    animation: showtext 0.4s;
}

.left h1{
    font-size: 36px;
    
}

.left h2{
    font-size: 36px;
    color: #fff;
    
}

.left p{
    font-size: 20px;
    font-weight: 300;
    color: #fff;
    
}

.right{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 0px 20px;
    border-left: 3px solid #FFC857;
    border-radius: 20px;
}

/* .envelope-container:hover{
    transform: translate(6px, 6px);
    box-shadow: 0px 0px #1B3F5A;
} */

@keyframes showtext {
    0%{
        opacity: 0;
    }
    100%{
        opacity: 100%;

    }
}

@keyframes envelope-move {
    0%{
        transform: translateY(0px);
    }
    50%{
        transform: translateY(12px);
    }
    100%{
        transform: translateY(0px);

    }
}


.loader {
    z-index: 2px;
    width: 80px; 
    height: 80px; 
}

.chart {
    width: 100%;
    height: 100%;
}

.circle {
    stroke-dasharray: 50;
    stroke-dashoffset: 0;
}

@keyframes dash {
  0% {
    stroke-dasharray: 0, 200;
    stroke-dashoffset: 0;
  }
  50% {
    stroke-dasharray: 100, 200;
    stroke-dashoffset: -15;
  }
  100% {
    stroke-dasharray: 100, 200;
    stroke-dashoffset: -120;
  }
}


</style>