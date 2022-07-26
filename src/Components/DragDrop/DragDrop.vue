<script setup>
import { reactive } from "vue";
import Input from "./Input.vue";

let boards = reactive([

])

function handleNewItem(text, board) {
    console.log(text.value, board.id, board.name);
    board.items.push({
        id: crypto.randomUUID(),
        title: text.value
    });
}

function handleNewboard() {
    const name = prompt("Nombre del board");
    boards.push({
        id: crypto.randomUUID(),
        name: name,
        items: []
    });
}

function startDrag(evt, board, item) {
    evt.dataTransfer.setData("text/plain", JSON.stringify({
        board: board.id,
        item: item.id
    }));
}

function onDrop(evt, dest) {
    const data = JSON.parse(evt.dataTransfer.getData("text/plain"));
    const board = boards.find(b => b.id === data.board);
    const item = board.items.find(i => i.id === data.item);
    dest.items.push(item);
    board.items.splice(board.items.indexOf(item), 1);
}

</script>

<template>
    <nav>
        <ul>
            <li><a class="btn" @click.prevent="handleNewboard">Create board</a></li>
        </ul>
    </nav>
    <div class="boards-container">
        <div class="boards">
            <div class="board" @drop="onDrop($event, board)" @dragover.prevent @dragenter.prevent
                v-for="board in boards" :key="boards.id">
                <div class="board-title">• {{ board.name }}</div>
                <Input @on-new-item="(text) => handleNewItem(text, board)" />
                <div class="items">
                    <div class="item" draggable="true" @dragstart="startDrag($event, board, item)"
                        v-for="item in board.items" :key="item.id">
                        {{ item.title }}
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<style scoped>
nav {
    margin-bottom: 20px;
}

ul {
    list-style: none;
    padding: 0;
}

li {
    display: inline-block;
    margin-right: 10px;
}

li a {
    color: #2f2f2f;
    font-weight: bold;
    background: rgb(144, 220, 162);
    padding: 10px;
    border-radius: 5px;
    text-decoration: none;
    cursor: pointer;
}

li a:hover {
    background: rgb(101, 164, 115);
}

.boards {
    display: flex;
    gap: 10px;
}

.board {
    background: #2c2c2c;
    padding: 10px;
}

.board-title {
    font-size: 18px;
    font-weight: bold;
    color: #efefef;
}

.item {
    background: #1d1d1d;
    padding: 10px;
    margin: 10px;
    border-radius: 5px;
}
</style>