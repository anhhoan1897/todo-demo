<template>
    <div class="hello" >
        {{ count }}
        <div class="box p-4">
            <div class="form-group ml-5 mr-5 row">
                <div class="col-10">
                    <input class="form-control" autofocus v-model.trim="newTodo" @keyup.enter="Add" placeholder="">
                    <VueDatePicker v-model="date" range :partial-range="false" :enable-time-picker="false" />
                </div>
                <div class="col-2">
                    <button @click="AddNewTask" class="btn btn-primary">
                        Add task
                    </button>
                </div>
            </div>
        </div>
        <div>
            <table class="mt-3 ListTodos">
                <p v-if="toDos.length < 1">
                    Empty List
                </p>
                <tr v-for="item in toDos " :key="item.id" :class="{ completed: item.completed }">
                    <td>
                        <input class="mark" type="checkbox" v-model="item.completed" />
                        <span class="checkmark"></span>
                    </td>
                    <td class="ok">
                        <label @click="edit(item)">{{ item.title | capitalize }}</label>
                        <input v-if="editting == item && item.completed != true" v-model="item.title" :class="{}"
                            @keyup.escape="doneEdit" @keyup.enter="doneEdit">
                    </td>
                    <td width="20%">
                        <a @click="Delete(item)" title="Xóa" class="delete badge badge-danger">
                            x
                        </a>
                    </td>
                </tr>
            </table>
            <div class="m-5 text-left">
                <b> Bạn có {{ allTasks }} task </b>
                <span class="badge badge-warning">
                    Task còn lại: {{ notDone }}
                </span>
                <span class="badge badge-success">
                    Task đã xong: {{ Done }}
                </span>
            </div>
        </div>
    </div>
</template>

<script>
import { capitalize, ref } from 'vue';
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'

const LOCAL_STORAGE_KEY = 'todo-app'

export default {
    name: "TodoList",
    components: { VueDatePicker },
    setup() {
        const date = ref();

        return {
            date
        }
    },

    data() {
        return {
            toDos: this.$store.state.toDos,
            newTodo: this.$store.state.newToDo,
            editting: this.$store.state.editting
        }
    },

    methods: {
        AddNewTask () {
            this.$store.dispatch('addTask', this.newTodo)
        },
        
    }

}
</script>