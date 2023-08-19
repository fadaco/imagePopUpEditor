<script setup>
  import modalView from './components/modalView.vue';
  import { ref } from 'vue'

  const isMove = ref(false)
  const mleftPosition = ref(null)
  const mtopPosition = ref(null)
  const mainClass = ref(null)
  const bgColor = ref("#b95e48")
  const items = ref([])
  const container = ref(null)
  const textItems = ref([])
  const imgItems = ref([])
  const openModal = ref(false)



const handleSwitch = (val) => {
    switch (val) {
        case 'BUTTON':
            return 400;
        case 'INPUT':
            return 350;
        case 'DIV':
            return 220;
        case 'P':
            return 550;
        case 'IMG':
            return 200;
        default:
            return 50;
   }
}


const mouseMove = (e) => {
    e.target.style.position = 'relative';
    mainClass.value = e.target;
    e.target.classList.add('is-edit')
    isMove.value = true;
    document.onmousemove = (evt) => {
        if (isMove.value) {
            mleftPosition.value = evt.pageX - handleSwitch(e.target.nodeName);
            mtopPosition.value = evt.pageY - handleSwitch(e.target.nodeName);

            e.target.style.left = mleftPosition.value + 'px';
            e.target.style.top = mtopPosition.value + 'px';
        }
    }

}

const mouseUp = () => {
    isMove.value = false;
    mainClass.value.classList.remove('is-edit')
    mleftPosition.value = mtopPosition.value = null
}

const editTextMode = (e) => {
    e.target.setAttribute('contenteditable', true);
}

const preview = () => {
  localStorage.removeItem('page')
  localStorage.setItem('page', JSON.stringify(container.value.innerHTML))
    openModal.value = true; 
}

const closeModal = () => {
  localStorage.removeItem('page');
  openModal.value = false;
}

const addElement = (val) => {
    if (val === 'button') {
        items.value.push('button')
    } else if (val === 'image') {
        imgItems.value.push('image')
    } else {
        textItems.value.push('text')
    }
}


const uploadImage = (e) => {
    var reader = new FileReader();
   reader.readAsDataURL(e.target.files[0]);
   reader.onload = function () {
       imgItems.value.push(reader.result)
   };
   reader.onerror = function (error) {
     throw(error)
   };
}


 
</script>

<template>
  <div>
  <div :style="{height: '100%', width: '900px', display: 'flex', padding: '20px 0', position: 'fixed', top: 0, left: 0, right: 0, bottom: 0}">
    
    

    <div ref="container" :style="{ background: bgColor, height: '600px', width: '600px', color: '#ffffff', padding: '20px', textAlign: 'center', position: 'fixed', left: '100px', margin: '0 auto', borderRadius: '50%'}" @mouseup="mouseUp">
        <div :style="{  background: 'transparent', border: '4px solid white', width: '100%', height: '100%', borderRadius: '50%', padding: '20px'}">
            <div :style="{display: 'flex', justifyContent: 'center', alignItems: 'center'}">
                <img @mousedown="mouseMove" src="/img.png" alt="star" width="40"/>
                <img @mousedown="mouseMove" :style="{marginBottom: '30px'}" src="/img.png" alt="star" width="70"/>
                <img @mousedown="mouseMove" src="/img.png" alt="star" width="40"/>
            </div>
        <div :style="{ cursor: 'crosshair', fontSize: '30px', fontWeight: 'bold', width: 'fit-content', margin: '0 auto'}" @mousedown="mouseMove" @dblclick="editTextMode" contenteditable="false">All the text and elements</div>
        <div :style="{ cursor: 'crosshair', fontSize: '30px', fontWeight: 'bold', width: 'fit-content', margin: '0 auto'}" @mousedown="mouseMove"  @dblclick="editTextMode" contenteditable="false">in this popup should be</div>
        <div :style="{ cursor: 'crosshair', fontSize: '30px', fontWeight: 'bold', width: 'fit-content', margin: '0 auto'}" @mousedown="mouseMove"  @dblclick="editTextMode" contenteditable="false">editable and dragable</div>

        <input :style="{cursor: 'crosshair', height: '50px', width: '80%', borderRadius: '10px', border: 'none' }" @mousedown="mouseMove" type="text" placeholder="E-mail" />
       <button :style="{cursor: 'crosshair', width: '80%', padding: '15px 0', fontSize: '30px', fontWeight: 'bold', borderRadius: '10px', margin: '20px 0'}"  @mousedown="mouseMove"  @dblclick="editTextMode">SIGNUP NOW</button>
       <p :style="{cursor: 'crosshair', fontSize: '20px', fontWeight: 'bold', width: 'fit-content', margin: '0 auto'}" @mousedown="mouseMove" contenteditable="false"  @dblclick="editTextMode">No credit card required. No Surprises</p>
       <ul class="btn">
        <li v-for="(item, index) in items" :key="index">
            <button :style="{cursor: 'crosshair', fontSize: '20px', fontWeight: 'bold', width: 'fit-content', margin: '0 auto'}" @mousedown="mouseMove" @dblclick="editTextMode">add btn</button>
        </li>
       </ul>
       <ul class="imgs">
        <li v-for="(item, index) in imgItems" :key="index">
            <img @mousedown="mouseMove" :src="item" alt="star" width="100"/>
        </li>
       </ul>
       <ul class="test">
        <li v-for="(item, index) in textItems" :key="index">
            <div :style="{cursor: 'crosshair', fontSize: '20px', fontWeight: 'bold', width: 'fit-content', margin: '0 auto'}"  @mousedown="mouseMove" @dblclick="editTextMode" contenteditable="false">add btn</div>
        </li>
       </ul>
    </div>
   </div>

   <div :style="{marginTop: '40px', position: 'fixed', left: '10px'}">
      <input :style="{width: '60px', height: '60px', marginBottom: '20px', fontWeight: 'bold', display: 'flex', justifyContent: 'center', alignItems: 'center'}" type="color" v-model="bgColor"/>
      <button :style="{width: '60px', height: '60px', marginBottom: '20px', fontWeight: 'bold', display: 'flex', justifyContent: 'center', alignItems: 'center'}" @click="preview">preview</button>
      <button :style="{width: '60px', height: '60px', marginBottom: '20px', fontWeight: 'bold', display: 'flex', justifyContent: 'center', alignItems: 'center'}" @click="addElement('button')">Add Button</button>
      <label for="image">
      <input id="image" type="file" @change="uploadImage" hidden />
        <div :style="{width: '60px', height: '60px', marginBottom: '20px', fontWeight: 'bold', display: 'flex', justifyContent: 'center', alignItems: 'center',  background: '#4d4c4e', color: '#ffffff', borderRadius: '5px'}">Upload Image</div>
      </label>
      <button :style="{width: '60px', height: '60px', marginBottom: '20px', fontWeight: 'bold', display: 'flex', justifyContent: 'center', alignItems: 'center'}" @click="addElement('text')">Add Text</button>
    </div>

   
  </div>
  <modalView :openModal="openModal" :bgColor="bgColor" @toggle-modal="closeModal"/>
  </div>
</template>

<style scoped>



    button, .upload-button {
        background: #4d4c4e;
        color: #ffffff;
        border-radius: 5px;
        border: none;
    }

    .is-edit {
        border: 1px dotted white;
        padding: 5px;
        
    }

   
</style>