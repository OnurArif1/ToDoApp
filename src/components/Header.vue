<template>
    <div class="container">
        <div class="row">

            <input type="text" v-model="newTodoText">
            <button @click="addToDo(newTodoText)">Add</button>
            <table class="table table-dark">
                <tr>
                    <th>Task</th>
                    <th>Checked</th>
                    <th>Delete</th>
                    <th>Edit</th>
                </tr>
                <tr v-for="(todo,index) in todos" :key="index" :class="{line:todo.checked}">
                    <td v-if="!todo.editMode">{{ todo.text }}</td>
                    <td v-else>
                        <input type="text" v-model="editedText" @keyup.enter="confirmEditedTask(todo,index)">
                    </td>
                    <td>
                        <input type="checkbox" v-model="todo.checked">
                    </td>
                    <td>
                        <button @click="deleteTask(index)">Delete</button>
                    </td>
                    <td>
                        <button @click="editTask(todo)">Edit</button>
                    </td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return{
            todos:[
                {
                    text:'onur',
                    checked:false,
                    editMode:false
                },
                {
                    text:'arif',
                    checked:false,
                    editMode:false
                }
            ],
            newTodoText:"",
            editedText:""
        }
    },
    mounted() { //LocalStorage sayesinde eklediğimiz verileeri local de tutabiliyoruz
        let newTodos = localStorage.getItem("todo");
        if (newTodos.length) {
        this.todos = JSON.parse(newTodos) || [];
        }
    },
    methods:{
        deleteTask(index){
            this.todos.splice(index, 1);
            localStorage.setItem("todo", JSON.stringify(this.todos));
        },
        addToDo(){
            if(this.newTodoText ==''){
                alert('herhangi bir şey yazmadınız!')
            }else{
                this.todos.push({
                    text:this.newTodoText,
                    checked:false,
                    editMode:false
                });
                localStorage.setItem("todo", JSON.stringify(this.todos));
                this.newTodoText = ''
            }
        },
        editTask(todo){
            todo.editMode=true;
        },
        confirmEditedTask(todo,index){
            this.todos[index].text=this.editedText
            todo.editMode=false;
            localStorage.setItem("todo", JSON.stringify(this.todos));
        }
    }
}
</script>