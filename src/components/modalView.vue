<script setup>
import { ref, onMounted, defineEmits, defineProps, onUpdated } from 'vue'

const preview = ref("");
const emit = defineEmits(['toggle-modal'])
const open = defineProps(['openModal', 'bgColor'])

onMounted(() => {
    preview.value = JSON.parse(localStorage.getItem('page'))

})

onUpdated(() => {
    preview.value = JSON.parse(localStorage.getItem('page'))
})

const toggleModal = () => {
    emit('toggle-modal')
}

const saveToLocalStorage = () => {
    localStorage.setItem('data', JSON.stringify(preview.value))
    toggleModal();
}


</script>

<template>
        <div class="overlay" :class="{'show-modal': open.openModal}" :style="{opacity: open.openModal, display: open.openModal ? 'block' : 'none'}">
            <div class="modal">
               <div class="button">
                <button @click="toggleModal">close</button>
                <button @click="saveToLocalStorage">Save</button>
               </div>
                <div v-if="preview" v-html="preview" :style="{ background: open.bgColor, height: '600px', width: '600px', color: '#ffffff', padding: '20px', textAlign: 'center', position: 'relative', margin: '0 auto', borderRadius: '50%'}"></div>

            </div>
        </div>
</template>

<style scoped>
    * {
        padding: 0;
        margin: 0;
    }

    .overlay {
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        z-index: 1; 
        background-color: rgba(0, 0, 0, 0.5);
    }

    .modal {
        max-width: 700px;
        height: 700px;
        margin: auto;
        background-color: white;
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        padding: 20px;
        border-radius: 15px;
    }

    .button {
        display: flex;
        justify-content: space-between;
    }

    button {
        border: none;
        background-color: transparent;
        font-size: 20px;
        cursor: pointer;
        font-weight: bold;
        color: green;
    }

    .button button:nth-child(1) {
        color: red;
    }
    .show-modal {
        opacity: 1;
        display: block;
        animation: show 0.2s; 
    }

    @keyframes show {
        from {
            opacity: 0;
            display: none;
            transform: scale(0);
        }
        to {
            opacity: 1;
            display: block;
            transform: scale(1);
        }
    }

    .hide-modal {
        opacity: 0;
        display: none;
        animation: hide .25s;
    }


    @keyframes hide {
        from {
            opacity: 1;
            display: block;
            transform: scale(1);
        }
        to {
            opacity: 0;
            display: none;
            transform: scale(0);
        }
    }

    @media(max-width: 768px) {
        .modal {
            width: 95%;
        }
    }
   
  


   
</style>