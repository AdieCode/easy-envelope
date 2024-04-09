<template>
    <div class="main-container">

        <!-- logo at top left of page -->
        <div id="logo">
            <img src="../images/logo-easy-envelope.png" alt="">
        </div>

        <!-- stack option in the top middle of the page -->
        <stack-bar :clickAction="hello" :stacks="stackData"/>

        <!-- sign out -->
        <sign-out name="Sign out" :width="184" :height="60" :textSize="20" :round="10" margin="10px 10px" @click="() => addNotification('hello')"/>
        

        <!-- the whole dashboard -->
        <div class="dashboard">

            <div class="top">
                <div id="welcome">
                    <h1>Hi, {{ name }}</h1>
                    <h2>Let’s start, shall we.</h2>
                </div>
                <div id="total-budget" v-if="stackData.length">
                    <h1>Envelope total</h1>
                    <h2>R {{ stackData[0].total_budget }}</h2>
                </div>
                <div class="options">
                    <my-button name="Delete" @click="toggleDelete"/>
                    <my-button name="Add" @click="toggleAddEnvelopeVisible"/>
                </div>
            </div>

            <div class="bottom">
                <template v-if="envelopeData.length">

                    <envelope v-for="(envelope, index) in envelopeData" :key="index" 
                    :category="envelope.category" 
                    :budget="envelope.current_budget" 
                    :description="envelope.description" 
                    :updateHandeler="() => toggleUpdateEnvelopeVisible(envelope.id)" 
                    :deleteHandeler="deleteEnvelope" 
                    :deleting="isDeleting"
                    :envelope_id="envelope.id"/>

                </template>

                <template v-else>
                    <p class="loading">Loading ... (This could take a while)</p>
                </template>
            </div>

        </div>

    </div>

    <!-- adding and updating envelopes -->
    <edit-envelope v-if="addEnvelopeObj.visible" :toggleVisibility="toggleAddEnvelopeVisible" :propertys="addEnvelopeObj"/>
    <edit-envelope v-if="updateEnvelopeObj.visible" :toggleVisibility="() => toggleUpdateEnvelopeVisible(selectedEnvelopeId)" :propertys="updateEnvelopeObj" />

    <!-- notifications of changes -->
    <notifications :notifications="notificationsList" v-if="notificationsList.length > 0"/>

    <!-- just a little bit of space at the bottom of the page -->
    <div id="page-bottom"></div>

</template>

<script setup>
import { ref, shallowRef } from 'vue';

let notificationsList = shallowRef([]);
const name = ref('Adriaan');
const envelopeData = ref([])
const stackData = ref([]);
let isDeleting = ref(false)

let selectedStack = ref([])
let selectedEnvelopeId = ref(0)

function hello(){
    name.value = 'Dirk';
}

function addNotification(message){
    notificationsList.value.push(message);
    console.log(notificationsList.value)
}

function toggleDelete(){
    isDeleting.value = !isDeleting.value
}

// function that handels envelope Add click
const toggleAddEnvelopeVisible = () => {
    addEnvelopeObj.value.visible = !addEnvelopeObj.value.visible;
};

// function that handels envelope update click
const toggleUpdateEnvelopeVisible = async(id) => {
    try {
        const envelope = await getEnvelope(id);
        selectedEnvelopeId.value = id;
        updateEnvelopeObj.value.id = id;
        
        updateEnvelopeObj.value.firstEdit = envelope[0].category;
        updateEnvelopeObj.value.secondEdit = envelope[0].current_budget.toString();
        updateEnvelopeObj.value.visible = !updateEnvelopeObj.value.visible;
    } catch (error) {
        console.error('Error fetching envelope:', error);
    }
};

const fetchData = async () => {
    try {
        const stacks = await getStacks(); // Wait for getStacks to resolve
        const envelopes = await getEnvelopes(); // Wait for getEnvelopes to resolve
        
        // Update reactive variables
        stackData.value = stacks;
        envelopeData.value = envelopes;
    } catch (error) {
        console.error('There was a problem with the fetch operation:', error);
    }
};

const getStacks = async () => {
    let responseData = {};

    try {
        const response = await fetch('https://personal-buget-manager-api-part-2.onrender.com/stacks');

        if (!response.ok) {
            throw new Error('Network response was not ok');
        }

        responseData = await response.json();

    } catch (error) {
      console.error('There was a problem with the fetch operation:', error);
    }
    
    return responseData;
}

const getEnvelopes = async () => {
    let responseData = {};

    try {
        const response = await fetch('https://personal-buget-manager-api-part-2.onrender.com/envelopes/1');

        if (!response.ok) {
        throw new Error('Network response was not ok');
        }

        responseData = await response.json();

    } catch (error) {
        console.error('There was a problem with the fetch operation:', error);
    }

    return responseData;
}

const getEnvelope = async (id) => {
    let responseData = {};

    try {
        const response = await fetch('https://personal-buget-manager-api-part-2.onrender.com/envelopes/envelope/' + id);

        if (!response.ok) {
        throw new Error('Network response was not ok');
        }

        responseData = await response.json();

    } catch (error) {
        console.error('There was a problem with the fetch operation:', error);
    }

    return responseData;
}

const addEnvelope = async (stack_id, category, budget) => {
    const envelopeToBeAdded = {
        stack_id: stack_id,
        category: category,
        description: "something",
        budget: budget
    };
    
    try {
        const response = await fetch('https://personal-buget-manager-api-part-2.onrender.com/envelopes/Add', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(envelopeToBeAdded)
        });

        if (!response.ok) {
            throw new Error('Network response was not ok');
        }

        const responseData = await response.json();
        fetchData();
        addNotification('Envelope Added')
        return responseData;
    } catch (error) {
        console.error('There was a problem with the fetch operation:', error);
        throw error; // Rethrow the error for handling outside
    }
};

const updateEnvelope = async (envelope_id, category, budget) => {
    const categoryToBeUpdated = {
        category: category
    };

    const budgetToBeUpdated = {
        budget: budget
    };
    
    try {
        const response1 = await fetch('https://personal-buget-manager-api-part-2.onrender.com/envelopes/editCategory/' + envelope_id, {
            method: 'PUT',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(categoryToBeUpdated)
        });

        const response2 = await fetch('https://personal-buget-manager-api-part-2.onrender.com/envelopes/editAmount/' + envelope_id, {
            method: 'PUT',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(budgetToBeUpdated)
        });

        if (!response1.ok && !response2.ok) {
            throw new Error('Network response was not ok');
        }
        fetchData();
        addNotification('Envelope Updated')

    } catch (error) {
        console.error('There was a problem with the fetch operation:', error);
        throw error; // Rethrow the error for handling outside
    }
};

const deleteEnvelope = async (envelope_id) => {
    
    try {
        const response = await fetch('https://personal-buget-manager-api-part-2.onrender.com/envelopes/' + envelope_id, {
            method: 'DELETE'
        });

        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        fetchData();
        addNotification('Envelope Deleted')

    } catch (error) {
        console.error('There was a problem with the fetch operation:', error);
        throw error; 
    }
};

const addEnvelopeObj = ref({
    firstEdit: '',
    secondEdit:'',
    type: 'Add',
    visible: false,
    id: 1,
    handler: addEnvelope
});

const updateEnvelopeObj = ref({
    firstEdit: '',
    secondEdit:'',
    type: 'Update',
    visible: false,
    id: 0,
    handler: updateEnvelope
});

let intervalId;
function popListInterval() {
  clearInterval(intervalId); // Clear any previous interval

  intervalId = setInterval(() => {
    if (notificationsList.value.length > 0) {
        notificationsList.value = notificationsList.value.filter((_, i) => i !== 0);
      console.log(notificationsList.value)
    }
  }, 10000); // 10 seconds
}

onMounted(() => {
    popListInterval();
    fetchData();
});

onUpdated(() => {
    // fetchData();
    
});
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
    overflow-x: hidden;
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

@keyframes showtext {
    0%{
        opacity: 0;
    }
    100%{
        opacity: 100%;

    }
}

</style>