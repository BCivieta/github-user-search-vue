<template>
    <div class="componente">
        <div class="buscador">
            <label class="bloque" for="usuario">Introduce un nombre de usuario:</label>
            <input type="text" id="ususario" :disabled="campoBusquedaDesactivado" @keydown.enter="obtenerUsuario">
            <p v-if="mostrarError" class="mensajeError">El nombre de usuario no existe</p>
        </div>
        <div class="datos">
            <div v-if="mostrarDatos" class="datos1">
                <img :src="avatar" alt="avatar del usuario">
                <div class="login">{{ login }}</div> 
                <div class="urlYboton">
                    <a :href = "githubUrl" target="_blank">URL de git Hub</a>
                    <button id="repositorios" @click="obtenerRepos">Ver repositorios</button>
                </div>
                <div class="mensajeError" v-if = "mostrarErrorRepos">No se han encontrado repositorios</div>
            </div>
            <div v-if="mostrarRepos" class="datos2">
                <GitHubRepo v-for="repo in repos" :key="repo.id" :repo="repo"></GitHubRepo>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import GitHubRepo from './GitHubRepo.vue';

const mostrarError = ref(false);
const mostrarErrorRepos = ref(false);
const mostrarDatos = ref(false);
const mostrarRepos = ref(false);
const campoBusquedaDesactivado = ref(false);
const usuario = ref('');
const avatar = ref('');
const login = ref('');
const githubUrl = ref('');
const repos = ref([]);

async function obtenerUsuario(event) {
    usuario.value = event.target.value;
    campoBusquedaDesactivado.value = true;
    try{
        const respuesta = await fetch (`https://api.github.com/users/${usuario.value}`)
        if(!respuesta.ok){
            throw new Error ("Usuario no encontrado");
        }
        const datos = await respuesta.json();
        avatar.value = datos.avatar_url;
        login.value = datos.login;
        githubUrl.value = datos.html_url;
        mostrarDatos.value = true;
        mostrarError.value = false;
        campoBusquedaDesactivado.value = false;
        mostrarRepos.value = false;
    }
    catch (error){
        mostrarError.value = true;
        mostrarDatos.value = false;
    }   
}
async function obtenerRepos(event) {
    try{
        const respuesta = await fetch(`https://api.github.com/users/${usuario.value}/repos`)
        console.log("Estado de la respuesta:", respuesta.status);
        if(!respuesta.ok){
            throw new Error("Repositorios no encontrados");
        }
        const datos = await respuesta.json();
        repos.value = datos;
        mostrarRepos.value = true;
        mostrarErrorRepos.value = false;
    }catch(error){
        mostrarErrorRepos.value = true;
        mostrarRepos.value = false;
    }
}
</script>

<style scoped>
.componente{
font-family: Verdana, Geneva, Tahoma, sans-serif;
display:flex;
flex-direction: column;
width: 100%;
align-items: center;
justify-content: center;
}
.buscador{
    margin-bottom: 20px;
    width: 94%;
    background-color: rgb(104, 143, 244);
    border-radius: 10px;
    border:solid 1px  rgb(11, 77, 243);
    padding: 20px;
    color: white;
}
.bloque{
    display: inline-block;
    width: 50%;
    text-align: right;
    margin-right: 5px;
    
}
input{
    text-align: left;
    width: 200px;
    border-radius: 5px;
    border-color: rgb(11, 77, 243);
    border-width: 1px;
    padding:6px;
}
.mensajeError{
    margin-top: 10px;
    color:white;
    border: solid 1px red;
    border-radius: 10px;
    background-color:rgb(243, 111, 111);
    width: 90%;
    text-align: center;
    padding: 15px;
}
.login{
    margin-top: 10px;
}
.urlYboton{
    width: 100%;
}
a{
    text-decoration: none;
    margin-top: 10px;
    color:blue
}
a:hover{
    color:rgb(217, 47, 47);
}
button{
    margin-top: 10px;
    border-radius: 10px;
    width: 150px;
    height: 30px;
    margin-left:20px;
}
.datos{
    display:flex;
}
.datos1{
    flex-direction: row;
    border:solid 1px black;
    border-radius: 10px;
    padding: 50px;
}
.datos2{
    flex-direction: row;
    width: 60%;
}
</style>
