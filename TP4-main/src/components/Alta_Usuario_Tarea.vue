
<template>
    
    
    <form>
        <h3>Alta:</h3>
        <br>
          N° ID Usuario:
        <select v-model="idu" @change="cambiarDescripcionU">
            <option v-for="opcion in BaseU" :key="opcion.id" :value="opcion.id" >
                {{ opcion.id }}
            </option>
        </select>
          Usuario:<input v-model="descripcionU" id="t1u">
          <br>  
          <br>  
          N° ID Tarea:
        <select v-model="idt" @change="cambiarDescripcion">
            <option v-for="opcion in Base" :key="opcion.id" :value="opcion.id" >
                {{ opcion.id + "-" + opcion.descripcion }}
            </option>
        </select>
          Tarea:<input v-model="descripcion" id="t1">
          <br>
          <br>  
          Estado:
          <select v-model="ide" @change="cambiarDescripcion">
            <option v-for="opcion in BaseE" :key="opcion.id" :value="opcion.id" >
                {{ opcion.id + "-" + opcion.descripcion}}
            </option>
        </select>
        <br>
        <br>  
          <button @click="crear()">Crear</button>
          <button>Salir</button>
    
    </form>
    
</template>

<style scoped>
   form {
    color: black;
    background-color: rgb(75, 82, 148);
    border-style: solid;
    border-color: black
   }
   h3{
    color: white;
    background-color: rgb(1, 1, 26);
   }
</style>

<script setup>
    import { ref, onMounted } from 'vue';

const idt = ref('');
const idu = ref('');
const ide = ref('');
const estado = ref('');
const Base = ref([]);
const BaseU = ref([]);
const BaseE = ref([]);
const descripcion = ref('');
const descripcionU = ref('');
const descripcionE = ref('');

const crear = async () => {
    const options = {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            id_usuario: idu.value,
            id_tarea: idt.value,
            id_estado: ide.value,
        })
    };
        
    const response = await fetch('http://localhost:3000/asignar/', options);
    if (response.ok) {
        alert('Tarea asignada');
    } else {
        alert('Error al asignar la tarea');
    }
};

onMounted(async () => {
    const options = {
        method: 'GET',
        headers: {
            'Content-Type': 'application/json'
        }
    };
    const response = await fetch('http://localhost:3000/tareas', options);
    Base.value = await response.json();
    const responseUsuario = await fetch('http://localhost:3000/usuarios', options);
    BaseU.value = await responseUsuario.json();
    const responseEstado = await fetch('http://localhost:3000/estados', options);
    BaseE.value = await responseEstado.json(); 
});
    
const cambiarDescripcion = () => {
    
    const selectedOption = Base.value.find(opcion => opcion.id === idt.value);
    
    if (selectedOption) {
        descripcion.value = selectedOption.descripcion;
    }
}

const cambiarDescripcionU = () => {
    
    const selectedOption = BaseU.value.find(opcion => opcion.id === idu.value);
    
    if (selectedOption) {
        descripcionU.value = selectedOption.nombre + " "+selectedOption.apellido;
    }
}
    

</script>