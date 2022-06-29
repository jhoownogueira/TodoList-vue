<template>
    <div class="todolist">
        <h1>My To-do List</h1>
        <input type="text" placeholder="Digite sua tarefa" @change="addToList" v-model="text">
        <ul>
            <li v-for="(item, index) in list" :key="index">
                <div class="tarefas" @click="toggleCheckbox(item)">
                    <input type="checkbox" :checked="item.done">
                    <p :class="{'done' : item.done}">{{item.label}}</p>
                </div>
                <span @click="deleteFromList(index)"><img src="../assets/trash-icon.svg" alt="">
                </span>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data() {
        return {
            list: [],
            text: ''
        }
    },
    created() {
        this.list = JSON.parse(localStorage.getItem('list')) || [];
    },
    methods: {
        addToList() {
            this.list.unshift({label: this.text, done: false});
            this.updateLocalStorage();
            this.text = '';
        },
        deleteFromList(index) {
            this.list.splice(index, 1);
            this.updateLocalStorage();
        },
        updateLocalStorage() {
            localStorage.setItem('list', JSON.stringify(this.list));
        },
        toggleCheckbox(item) {
            item.done = !item.done;
            this.updateLocalStorage();
        }
    }
}

</script>

<style>
    .todolist {
        margin: 0 auto;
        width: 100%;
        max-width: 80rem;
        padding: 0 3.2rem;
    }
    h1 {
        font-size: 3.2rem;
        text-align: center;
        color: black;
        font-weight: bold;
    }
    input[type="text"] {
        width: 100%;
        height: 4.8rem;
        margin: 1.6rem 0;
        padding: 1.2rem;
        font-size: 1.6rem;
        border-radius: 1.2rem;
        border: 1px solid lightgray;
    }

    input[type="text"]::placeholder {
        color: rgba(0, 0, 0, 0.5);
    }
    ul {
        width: 100%;
        padding: 0 1rem;
    }

    ul li {
        display: flex;
        justify-content: space-between;
    }

    ul li span {
        transition: all 0.3s;
    }

    ul li span:hover {
        transform: scale(1.05) rotate(5deg);   
    }

    ul li span:active {
        transform: scale(1) rotate(-5deg);
    }

    .tarefas {
        width: 100%;
        display: flex;
    }

    .tarefas p {
        color: black;
        font-size: 1.6rem;
    }

    .tarefas input[type="checkbox"] {
        min-width: 2.4rem;
        height: 2.4rem;
        margin-right: 1.2rem;
    }
    .done {
        text-decoration: line-through;
        opacity: 0.5;
    }
</style>