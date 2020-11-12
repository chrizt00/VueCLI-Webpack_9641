<template>
    <v-main class="list">
        <h3 class="text-h3 font-weight-medium mb-5">To Do List</h3>

        <v-card>
            <v-card-title primary-title>
                <v-text-field
                v-model="search"
                append-icon="mdi-magnify"
                label="search"
                single-line
                hide-details></v-text-field>
                <v-spacer></v-spacer>
                
                <v-btn color="success" dark @click="dialog = true">Tambah</v-btn>
            </v-card-title>
            <v-data-table :headers="headers" :items="todos" :search="search" pagination.sync="pagination">
                <template v-slot:[`item.actions`]="{ item }">
                    <v-btn small class="mr-2" @click="editItem(item)">
                    edit
                    </v-btn>
                    <v-btn small @click="deleteItem(item)">
                    delete
                    </v-btn>
                </template>
            </v-data-table>
        </v-card>

        <v-dialog
            v-model="dialog"
            persistent
            max-width="600px"
            transition="dialog-transition"
        >
            <v-card>
                <v-card-title>
                    <span class="headline">From Todo</span>
                </v-card-title>
                <v-card-text>
                    <v-container>
                        <v-text-field
                            v-model="formTodo.task"
                            label="Task"
                            required
                        ></v-text-field>
                        <v-select
                            :items="['Penting', 'Biasa', 'Tidak Penting']"
                            v-model="formTodo.priority"
                            label="Priority"
                            required
                        ></v-select>
                        <v-textarea
                        v-model="formTodo.note" label="Note" required>
                        </v-textarea>
                    </v-container>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="cancel">Cancel</v-btn>
                    <v-btn color="blue darken-1" text @click="save">Save</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </v-main>
</template>
<script>
export default {
    name:"List",
    data(){
        return {
            search:null,
            dialog:false,
            headers: [
                {
                    text:'Task',
                    align: 'start',
                    sortable: true,
                    value: 'task',
                },
                { text: "Priority", value:"priority"},
                { text: "Note", value:"note"},
                { text: "Actions", value:"actions"},
            ],
            todos: [
                {
                    task: "bernafas",
                    priority: "Penting",
                    note: "hufftts",
                },
                {
                    task: "nongkrong",
                    priority: "Tidak Penting",
                    note: "bersama teman teman"
                },
            ],
            formTodo:  {
                task: null,
                priority: null,
                note: null,
            },
        };
    },
    methods: {
        save(){
            let index=  this.findIndexTodos(this.formTodo);
            if(index < 0){ // alias ga ketemu -1
                this.todos.push(this.formTodo);
            }else {
                this.todos[index] = this.formTodo;
            }
            this.resetForm();
            this.dialog = false;
        },
        cancel(){
            this.resetForm();
            this.dialog = false;
        },
        resetForm(){
            this.formTodo = {
                task: null,
                priority: null,
                note: null,
            };
        },
        editItem(item){
            let index = this.findIndexTodos(item);
            this.formTodo = item;
            this.dialog = true;
        },
        deleteItem(item){
            let x = window.confirm("Apa yakin ingin menghapus?  ");
            if(x === true){
                let index = this.findIndexTodos(item);
                this.todos.splice(index,1);
            }
        },
        findIndexTodos(item){
            return this.todos.findIndex(obj => obj.task === item.task);
        }
    },
};
</script>