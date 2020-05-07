
<template>
    <div>
        <h1>Usuarios</h1>

        <table class="table table-hover">
            <thead>
                <tr>
                    <td>ID</td>
                    <td>Nombre(s)</td>
                    <td>Apellido</td>
                    <td>Acciones</td>
                </tr>
            </thead>

            <tbody>
                <tr v-for="user in users" :key="user.id">
                    <td>{{ user.id }}</td>
                    <td>{{ user.firstName }}</td>
                    <td>{{ user.lastName }}</td>
                    <td><router-link :to="{name: 'Edit', params: { id: user.id }}" class="btn btn-primary">Editar</router-link></td>
            <td><button class="btn btn-danger"  v-on:click="deleteUser(user)">Borrar</button></td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    import Users from '../../data/static/users.json';
    export default {
        data() {
            return{
                users: []
            };
        },

        created: function ()
        {
            this.getUsers();
        },

        methods: {
            getUsers()
            {
                let uri = 'http://localhost:4000/users';
                this.axios.get(uri).then((response) => {
                    this.users = response.data;
                }).catch(error => {
                    console.log("error: " + error);
                    this.users = Users;
                });
            },
            deleteUser(user)
            {
                let uri = 'http://localhost:4000/items/delete/' + user.id;
                let index = -1;
                this.axios.get(uri).then(response => {
                    console.log("response: " + response);
                    index = this.users.indexOf(user);
                    if (index !== -1) {
                        this.users.splice(index, 1);
                    }
                }).catch(error => {
                    console.log("error: " + error);
                    index = this.users.indexOf(user);
                    if (index !== -1) {
                        this.users.splice(index, 1);
                    }
                });
            }
        }
    }
</script>