<template>
    <div class="componente">
        <div class="repo" :title="repo.description || 'No hay descripción'">
            <h5 >{{ repo.full_name }}</h5>
            <a :href= "repo.html_url" target="_blank">Accerder desde GitHub</a>
            <div class="fork">
                {{ repo.forks_count }}<p>forks</p>
            </div>
        </div>
    </div>
</template>
<script setup>
import { defineProps } from 'vue';
defineProps({
    repo:Object
});
</script>
<style scoped>
.componente{
    width: 100%;
    display: flex;
    border:solid 1px black;
    border-radius: 10px;
    margin-bottom:10px;
    margin-left: 10px;
    justify-content: left;
    text-align: left;
}
.repo{
    display: flex;
    flex-direction: row;
    padding: 10px;
    padding-left: 10px;
    align-items: center;
    width: 100%;
}
h5{
    margin-left:10px
}
a{
    color:black;
    margin:10px;
}
a:hover{
    color:red;
    
}
.fork{
    border-radius:10px;
    background-color: rgb(57, 108, 238);
    color:white;
    display: flex;
    justify-content: center;
    width: 60px;
    padding: 5px;
    margin-left: auto;
    align-items: center;
    margin-right: 10px;
}
p{
    font-size: 10px;
    margin-left: 3px;
}
</style>