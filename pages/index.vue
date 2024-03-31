<template>
    <div class="main-container">

        <!-- logo at top left of page -->
        <div id="logo">
            <img src="../images/logo-easy-envelope.png" alt="">
        </div>

        <!-- stack option in the top middle of the page -->
        <stack-bar :clickAction="hello" :stacks="stackData"/>

        <!-- sign out -->
        <sign-out name="Sign out" :width="184" :height="60" :textSize="20" :round="10" margin="10px 10px"/>
        

        <!-- the whole dashboard -->
        <div class="dashboard">

            <div class="top">
                <div id="welcome">
                    <h1>Hi, {{ name }}</h1>
                    <h2>Let’s start, shall we.</h2>
                </div>
                <div id="total-budget" v-if="stackData.length">
                    <h1>Envelope total</h1>
                    <h2>${{ stackData[1].total_budget }}</h2>
                </div>
                <div class="options">
                    <my-button name="Delete"/>
                    <my-button name="Add"/>
                </div>
            </div>

            <div class="bottom">
                <template v-if="envelopeData.length">
                    <envelope v-for="(envelope, index) in envelopeData" :key="index" :category="envelope.category" :budget="envelope.current_budget" :description="envelope.description" />

                    <envelope v-for="(envelope, index) in envelopeData" :key="index" :category="envelope.category" :budget="envelope.current_budget" :description="envelope.description" />
                </template>
                <template v-else>
                    <p class="loading">Loading ... (This could take a while)</p>
                </template>
            </div>

        </div>

    </div>

    <div id="page-bottom"></div>

</template>

<script setup>
import { ref } from 'vue';


function hello(){
    name.value = 'Dirk';
}

const name = ref('Adriaan');
const envelopeData = ref([])
const stackData = ref([])

const fetchData = async () => {
    try {
        console.log('fetching from server...')
      const response = await fetch('https://personal-buget-manager-api-part-2.onrender.com/stacks');
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      const responseData = await response.json();
      stackData.value = responseData;
    } catch (error) {
      console.error('There was a problem with the fetch operation:', error);
    }

  try {
    console.log('fetching from server...')
    const response = await fetch('https://personal-buget-manager-api-part-2.onrender.com/envelopes/1');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const responseData = await response.json();
    envelopeData.value = responseData;
  } catch (error) {
    console.error('There was a problem with the fetch operation:', error);
  }

};

onMounted(fetchData);
</script>

<style lang="css">
@import url('https://fonts.googleapis.com/css2?family=Newsreader:ital,opsz,wght@0,6..72,200..800;1,6..72,200..800&display=swap');

*{
    padding: 0;
    margin: 0;
}

body{
    font-family: "Newsreader", serif;
    font-optical-sizing: auto;
    font-style: normal;
    color: #FFC857;
    background-color: #1F2041;
}
</style>

<style scoped>

.main-container{
    display: flex;
    justify-content: space-evenly;
    align-content: center;
}

#logo{
    position: absolute;
    top: 20px;
    left: 20px;
    cursor: pointer;
}
#logo img{
    width: 250px;
    height: auto;

}



.dashboard{
    margin-top: 180px;
    min-width: 1300px;
    min-height: 730px;
    height: auto;
    width: 60%;
    /* background-color: blue; */
}
.top{
    display: flex;
    justify-content: space-between;
    align-items: end;
    height: auto;
}

.loading{
    animation: loading 2s infinite;
    font-size: 24px;
    font-weight: 900;
}

@keyframes loading {
    0%{
        transform: translateX(0);
    }
    25%{
        transform: translateX(10px);
    }
    50%{
        transform: translateX(-10px);
    }
    75%{
        transform: translateX(10px);
    }
    100%{
        transform: translateX(0);
    }
}
#welcome{
    margin-bottom: 20px;

}

#welcome h1{
    font-size: 48px;
    font-weight: 600;  
    margin-bottom: 15px;
}

#welcome h2{
    font-size: 36px;
    font-weight: 300;
}

#total-budget{
    display: flex;
    flex-direction: column;

    opacity: 100%;
    animation: showtext 0.2s;
}

@keyframes showtext {
    0%{
        opacity: 0;
    }
    100%{
        opacity: 100%;

    }
}


#total-budget h1{
    color: #fff;
    font-size: 32px;
    font-weight: 400;
    margin: auto;
    margin-bottom: 20px;
}

#total-budget h2{
    font-size: 64px;
    font-weight: 600; 
    margin-bottom: 20px; 
}

.options{
    display: flex;
    /* flex-direction: row; */
}

.bottom{
    background-color: #4b3f727b;
    border-radius: 20px;
    border: 40px solid #4b3f7200;
    height: auto;
    display: flex;
    justify-content: start;
    align-items: start;
    flex-wrap: wrap;
    gap: 20px;
    position: relative;
    
}

#page-bottom{
    min-height: 100px;
}
   
    
</style>