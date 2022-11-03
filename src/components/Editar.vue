<template>
    <div class="container">

        <div class="card">
            <div class="card-header">
                Editar empleado
            </div>
            <div class="card-body">

                <form v-on:submit.prevent="actualizarRegistro">
                    <div class="form-group">
                      <label for="nombre">Nombre:</label>
                      <input type="text"
                        class="form-control" required  name="nombre" v-model="empleado.nombre" id="nombre" aria-describedby="helpId" placeholder="">
                      <small id="helpId" class="form-text text-muted">Escribe el nombre</small>
                    </div>
                    <div class="form-group">
                      <label for="correo">Correo:</label>
                      <input type="email"
                        class="form-control" required name="correo" v-model="empleado.correo" id="correo" aria-describedby="helpId" placeholder="">
                      <small id="helpId" class="form-text text-muted">Digita el Correo</small>
                    </div>
                    <div class="btn-group" role="group" aria-label="">
                        <button type="submit" class="btn btn-primary mx-2">Actualizar</button>
                        <router-link :to="{name:'Listar'}" class="btn btn-warning">Cancelar</router-link>

                    </div>
                </form>

            </div>
        </div>

    </div>
</template>

<script>
    export default {
                /* Apartado para enviar informacion con js puro */
                data(){
            /* empleado de data es el espacio, o especie de array en donde guardaremos los datos */
            return{
                empleado:{}
            }
        },
        created:function(){
            this.obtenerInfoID();
        },
        methods:{
            obtenerInfoID(){

                fetch('http://localhost/empleados/?consultar='+this.$route.params.id)
                .then(respuesta=>respuesta.json())
                .then((datosRespuesta)=>{

                    console.log(datosRespuesta);
                    this.empleado=datosRespuesta[0];

                })
                .catch(console.log)
            },
            actualizarRegistro(){
                var datosEnviar={id:this.$route.params.id, nombre:this.empleado.nombre, correo:this.empleado.correo}

/*                     fetch es la forma en la que pasamos una url en este caso es la url de nuestra api respuesta 
                    json, como enviamos info es metodo post y la estructuramos en un JSON y guardamos el nombre
                    de la funcion */
/*                     El 1 es un parametro que dirije la informacion a codigo php
                    Nota: no entiendo porque es 1 pero es 1 jajajaj */
                    fetch('http://localhost/empleados/?actualizar='+this.$route.params.id,{
                        method: "POST",
                        body:JSON.stringify(datosEnviar) 
                    })
/*                     Luego de eso generamos la respuesta en donde se recibe su json, la funcion
                    para luego redirigirlo a una url en especifico */
                    .then(respuesta=>respuesta.json())
                    .then((datosRespuesta=>{
                            console.log(datosRespuesta);
                            window.location.href='../listar'
                    }))
            }
        }
    }
</script>