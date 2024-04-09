<template>
    <div class="notifications">
        <div class="notification"  v-for="(notification, index) in notificationsList" :key="index" @click="() => removeFromList(index)">

            Notification : {{ notification }} 

            <div class="cancel">
                <img src="../images/icons8-close-120.png" alt="">
            </div>

        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';

let notificationsList = ref([])

const props = defineProps({
    notifications: {type: Array, default: []}
})

function removeFromList(index) {
    notificationsList.value = notificationsList.value.filter((_, i) => i !== index);
    console.log(props.notifications);
}

let intervalId; // Define a variable to store the interval ID

function doSomthing() {
  setInterval(() => {

    notificationsList.value.push('');
    notificationsList.value.pop();

    
  }, 1000); // 10 seconds
}

onMounted(() => {
    notificationsList.value = props.notifications;
    doSomthing()
})

onUnmounted(() => {
    clearInterval(doSomthing);
})

</script>

<style lang="css" scoped>
.notifications{
    position: fixed;
    right: 20px;
    bottom: 20px;
}

.notification{
    width: 400px;
    height: 40px;
    background-color: #4b3f72c8;
    border-radius: 10px;
    border: 4px solid #FFC857;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px;
    margin-top: 20px;    
    font-size: 20px;
    font-weight: 700;
    animation: showNotification 0.4s,
               slideAway 0.6s 5s forwards;
}

.cancel{
    width: 40px;
    height: 100%;
    border-radius: 4px;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
}

.cancel:hover{
    background-color: #4b3f72;
}

.cancel img{
    width: 30px;
    height: auto;
}

@keyframes showNotification {
    0%{
        transform: translateX(500px);
    }
    60%{
        transform: translateX(-20px);

    }
    80%{
        transform: translateX(10px);

    }
    100%{
        transform: translateX(0px);

    }
}

@keyframes slideAway {
    0%{
        transform: translateX(0px);

    }
    60%{
        transform: translateX(-20px);
        
    }
    100%{
        transform: translateX(500px);
    }
}

@keyframes fadeNotification {
    0%{
        opacity: 100%;
    }
    100%{
        opacity: 0%;

    }
}
</style>