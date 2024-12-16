<template>
    <div id="page">
        <h1>Product Management</h1>
        <div>
            <input type="text" placeholder="Enter name" v-model="form.name">
            <input type="price" placeholder=0 v-model="form.price">
            <input type="checkbox" name="option1" :true-value="'On Shelf'" :false-value="'Off Shelf'"
                v-model="form.status">
            <button @click="save">Save</button>
            <button @click="clearForm">Cancel</button>
        </div>
        <div>
            <button @click="filter = 'all'">All</button>
            <button @click="filter = 'On Shelf'">On Shelf</button>
            <button @click="filter = 'Off Shelf'">Off Shelf</button>
        </div>
        <div>
            <table border="1">
                <thead>
                    <tr>
                        <th>Id</th>
                        <th>Name</th>
                        <th>Price</th>
                        <th>Status</th>
                        <th>Action</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="user in filterProducts" :key="user.id">
                        <td>{{ user.id }}</td>
                        <td>{{ user.name }}</td>
                        <td>{{ user.price }}</td>
                        <td>{{ user.status }}</td>
                        <td><button @click="editUser(user.id)">Edit</button><button
                                @click="deleteUser(user.id)">Delete</button></td>
                    </tr>
                    <tr v-if="users.length === 0">
                        <td colspan="5">No Data</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script setup lang='ts'>
import { ref, reactive, computed } from 'vue'
interface User {
    id: number
    name: string
    price: number
    status: string
}
const form = ref<User>({ id: 0, name: '', price: 0, status: 'Off Shelf' })
const users = ref<User[]>([])
const lastId = ref(1)
const editedIndex = ref<number | null>(null)
const filterProducts = computed(function () {
    if (filter.value === 'On Shelf') {
        return users.value.filter(function (item) {
            return item.status === 'On Shelf'
        })
    } if (filter.value === 'Off Shelf') {
        return users.value.filter(function (item) {
            return item.status === 'Off Shelf'
        })
    } else {
        return users.value
    }
})
const filter = ref<string>('all')

function save() {
    if (editedIndex.value !== null) {
        updateUser()
    } else {
        addUser()
    }
}
function editUser(id: number) {
    const index = users.value.findIndex(function (item) {
        return item.id === id
    })
    editedIndex.value = index
    form.value = { ...users.value[index] }
}
function updateUser() {
    if (editedIndex.value !== null) {
        users.value[editedIndex.value] = { ...form.value }
        clearForm()
        editedIndex.value = null
    }
}
function addUser() {
    users.value.push({ ...form.value, id: lastId.value })
    lastId.value++
    clearForm()
}
function deleteUser(id: number) {
    const index = users.value.findIndex(function (item) {
        return item.id === id
    })
    users.value.splice(index, 1)
}
function clearForm() {
    form.value = { id: 0, name: '', price: 0, status: 'Off Shelf' }
}

</script>

<style scoped>
#page {
    text-align: center;
}

table {
    margin: 20px auto;
    width: 80%;
    border-style: solid;
    border-collapse: collapse;
}

input,
button {
    margin-left: 4px;
}

td,
th {
    padding: 8px;
    text-align: center;
}
</style>