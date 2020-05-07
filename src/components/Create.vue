<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                <h3>Crear Usuario</h3>
            </div>
            <div class="card-body">
                <form v-on:submit.prevent="addUser" id="save-form">
                    <div class="form-group">
                        <label>Nombre:</label>
                        <input type="text" class="form-control" v-model="user.firstName"/>
                    </div>
                    <div class="form-group">
                        <label>Apellidos:</label>
                        <input type="text" class="form-control" v-model="user.lastName"/>
                    </div>
                    <div class="form-group">
                        <input type="submit" class="btn btn-primary" value="Guardar Usuario"/>
                    </div>
                    <div class="alert alert-success" role="alert" id="success-label" style="display: none">
                        <label>Usuario guardado con exito!</label>
                        <br/>
                        <label>Dentro de 3 segundos será redirigido a la pantalla donde podrá volver a ver todos los usuarios</label>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                user: {}
            };
        },
        methods: {
            addUser() {
                let uri = 'http://localhost:40000/items/add';
                console.log(this.user);
                this.axios.post(uri, this.user).then((response) => {
                    console.log(response.data);
                    if (response.data && response.data.success) {
                        let successDiv = document.getElementById("success-label");
                        if (successDiv) {
                            successDiv.style.display = "block";

                        }
                        let form = document.getElementById("save-form");
                        if (form) {
                            form.reset();
                        }
                        setTimeout(function () {
                            if (successDiv) {
                                successDiv.style.display = "none";
                            }
                        }, 3000);
                    }
                }).catch(error => {
                    console.log("error: " + JSON.stringify(error));
                    let successDiv = document.getElementById("success-label");
                    if (successDiv) {
                        successDiv.style.display = "block";

                    }
                    let form = document.getElementById("save-form");
                    if (form) {
                        form.reset();
                    }
                    setTimeout(function () {
                        if (successDiv) {
                            successDiv.style.display = "none";
                        }
                    }, 3000);
                });
            }
        }
    }
</script>