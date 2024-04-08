<template>

    <div class="main_div">
        <v-btn variant="outlined" @click="adddiv">
            Add More +
        </v-btn>
        <v-container fluid>
            <v-row no-gutters>
                <div v-if="colums.length" v-for="(col, index) in colums" :key="index">
                    <div id="draggable" @dragenter="drag(index)" draggable="true">
                        <div class="contain_div pl-16 pt-10"
                            :style="{ width: `${col?.width}px`, height: `${col?.height}px` }">
                            {{ col?.title }}
                            <span class="close_div" @click="remove(index)">&#128473;</span>
                        </div>
                    </div>
                </div>
                <div v-else>
                    <h1>Please add div....</h1>
                </div>
            </v-row>
        </v-container>
    </div>

</template>
<script setup lang="js">
import { reactive, onMounted } from 'vue'
let colums = reactive([])
let dragvalue = []
let dropvalue = 0

let drag = (index) => {
    if (dragvalue.length < 2) {
        dragvalue.push(index)
        if (dragvalue.length > 1) {
            dropvalue = colums.splice(dragvalue[0], 1)[0];
            setTimeout(rearrange, 900);
        }
    }
}
let adddiv = () => {
    const newColumn = {
        title: colums.length + 1,
        width: '200',
        height: '100',
    };
    colums.push(newColumn);
    localStorage.setItem('records', JSON.stringify(colums));
}

let rearrange = () => {
    colums.splice(dragvalue[1], 0, dropvalue);
    dragvalue.length = 0;
    localStorage.setItem('records', JSON.stringify(colums));
}

let remove = (index)=>{
    colums.splice(index, 1)[0];
    localStorage.setItem('records', JSON.stringify(colums));
}

onMounted(() => {
    if (localStorage.getItem('records') !== null) {      
        Object.assign(colums, JSON.parse(localStorage.getItem('records')))
    }
})
</script>

<style src="../../assets/css/main.css"></style>