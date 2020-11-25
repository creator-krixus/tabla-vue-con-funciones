<template>
    <div id = "app" class = "p-3">
        <b-btn @click = "isUserAdmin = !isUserAdmin" class="btn-admin">Admin</b-btn>
        <b-btn class="btn-add" @click.prevent = "form = !form">Add user</b-btn>
        <br /><br />
        <b-table striped hover :fields = "computedFields" :items = "items">

            <template v-slot:cell(show-buttons)="row">
                <b-btn class="btn" @click = "edit(row.index)" @click.prevent = "newCambio = !newCambio">Edit</b-btn>
                <b-btn class="btn" @click = "remove(row.index)">Delete</b-btn>
            </template>

        </b-table>

        <div v-bind = "form" v-if = "form">
            <form>
                <input v-model = "newFirstName" placeholder = "First Name"><br><br>
                <input v-model = "newLastName"  placeholder = "Last Name"><br><br>
                <input v-model = "newAge"       placeholder = "Age"><br><br>
                <input v-model = "newSex"       placeholder = "Sex"><br><br>

                <button class = "btn btn-dark" @click = "addUser">Save</button>
            </form>
        </div>
        <div v-bind = "newCambio" v-if = "newCambio">
            <form>
                <input v-model = "changeFirst" placeholder = "First Name"><br><br>
                <input v-model = "changeLast"  placeholder = "Last Name"><br><br>
                <input v-model = "changeAge"       placeholder = "Age"><br><br>
                <input v-model = "changeSex"       placeholder = "Sex"><br><br>

                <button @click="save">Save</button>

            </form>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data() {
            return {
                isUserAdmin: false,
                form: false,
                newCambio: false,
                selectedIndex: null,
                fields: [{
                        key: 'show-buttons',
                        label: 'Actions',
                        admin: true
                    },
                    {
                        key: 'first',
                        label: 'First Name',
                        sortable: true,
                    },
                    {
                        key: 'last',
                        label: 'Last Name',
                        sortable: true,
                    },
                    {
                        key: 'age',
                        label: 'Age',
                        sortable: true
                    },
                    {
                        key: 'sex',
                        label: 'Sex'
                    },

                ],
                items: [
                    {
                        
                        first: 'John',
                        last: 'Doe',
                        sex: 'Male',
                        age: 42
                    },
                    {
                        
                        first: 'Jane',
                        last: 'Doe',
                        sex: 'Female',
                        age: 36
                    },
                    {
                        
                        first: 'Rubin',
                        last: 'Kincade',
                        sex: 'Male',
                        age: 73
                    },
                    {
                        
                        first: 'Shirley',
                        last: 'Partridge',
                        sex: 'Female',
                        age: 62
                    }
                ],
                newFirstName: "",
                newLastName: "",
                newAge: "",
                newSex: "",

                changeFirst: "",
                changeLast: "",
                changeAge: "",
                changeSex: "",
            }
        },
        computed: {
            computedFields(){
                // If the user isn't an admin, filter out fields that require auth
                if (!this.isUserAdmin){
                    return this.fields.filter(field => !field.admin);
                // If the user IS an admin, return all fields.
                }else{
                    return this.fields;
                    }
            }  
        },
        methods: {
        addUser() {
            if (this.newFirstName != "" && this.newLastName != "" && this.newAge != "" && this.newSex != "") {
                this.items.unshift({
                    age: this.newAge,
                    first: this.newFirstName,
                    last: this.newLastName,
                    sex: this.newSex
                })
            } else {
                alert("Complete all form fields")
            }
        },
        remove(index){
            console.log(index);
            this.items.splice(index, 1);
        },
        edit(index){
            console.log(this.items);
            console.log(index);
            this.changeFirst = this.items[index].first
            this.changeLast = this.items[index].last
            this.changeAge = this.items[index].age
            this.changeSex = this.items[index].sex
            this.selectedIndex = index
            
        },
        save(){
            this.items[this.selectedIndex].first = this.changeFirst
            this.items[this.selectedIndex].last = this.changeLast
            this.items[this.selectedIndex].age = this.changeAge
            this.items[this.selectedIndex].sex = this.changeSex

            this.selectedIndex = null
            this.newCambio = false
        }
    }
}     
</script>
<style scoped>
    .btn {
        margin-right: 5px;
    }
    form {
        border: solid 1px;
        padding: 12px;
        width: max-content;
        margin-left: auto;
        margin-right: auto;
        border-radius: 5px;
        background: grey;
    }
</style>