
<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                <h3>Edit Item</h3>
            </div>
            <div class="card-body">
                <form v-on:submit.prevent="updateItem">
                    <div class="form-group">
                        <label>Nombre</label>
                        <input type="text" class="form-control" v-model="item.firstName"/>
                    </div>
                    <div class="form-group">
                        <label>Apellidos:</label>
                        <input type="text" class="form-control" v-model="item.lastName" />
                    </div>
                    <div class="form-group">
                        <input type="submit" class="btn btn-primary" value="Guardar Cambios"/>
                    </div>
                    <div class="alert alert-success" role="alert" id="success-label" style="display: none">
                        <label>Registro editado con exito!</label>
                        <br/>
                        <label>Dentro de 3 segundos será redirigido a la pantalla donde podrá volver a ver todos los usuarios</label>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>
<script>
    import Users from '../../data/static/users.json';
    export default{
        data() {
            return{
                item: {}
            };
        },

        created: function () {
            this.getItem();
        },

        methods: {
            getItem()
            {
                console.log("id route: " + this.$route.params.id);
                let uri = 'http://localhost:4000/items/edit/' + this.$route.params.id;
                let id = this.$route.params.id;
                this.axios.get(uri).then((response) => {
                    this.item = response.data;
                }).catch(error => {
                    console.log("error: " + error);
                    let users = Users;
                    let user = users.find(u => u.id === id);
                    if (user) {
                        this.item = user;
                    }
                });
            },

            updateItem()
            {
                let uri = 'http://localhost:4000/items/update/' + this.$route.params.id;
                this.axios.post(uri, this.item).then((response) => {
                    if (response.data && response.data.success) {
                        let successDiv = document.getElementById("success-label");
                        if (successDiv) {
                            successDiv.style.display = "block";
                            var that = this;
                            setTimeout(function () {
                                that.$router.push({name: 'Index'});
                            }, 3000);
                        }
                    }
                    this.$router.push({name: 'Index'});
                }).catch(error => {
                    console.log(JSON.stringify(error));
                    let successDiv = document.getElementById("success-label");
                    if (successDiv) {
                        successDiv.style.display = "block";
                        var that = this;
                        setTimeout(function () {
                            that.$router.push({name: 'Index'});
                        }, 3000);
                    }
                });
            }
        }
    }
</script>