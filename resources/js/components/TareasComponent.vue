<template>
    <div>
        <form @submit.prevent="agregar">
            <h3>Agregar nota</h3>
            <input type="text" class="form-control mb-2" placeholder="nombre de la nota" v.model="nota.nombre">
            <input type="text" class="form-control mb-2" placeholder="nombre de la nota" v.model="nota.nombre">
            <button class="btn btn-primary" type="submit">Agregar</button>
        </form>

        <hr class="mt-3">
        <h3>Listado de notas</h3>
        <ul class="list-group my-2">
            <li class="list-group-item" v-for="(item, index) in notas" :key="index">
                <span class="badge badge-primary float-right">
                    {{ item.updated_at }}
                </span>
                <p>{{ item.nombre }}</p>
                <p>{{ item.descripcion }}</p>
                <button class="btn btn-danger btn-sm" @click="eliminarNota(item, index)">Eliminar</button>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data() {
        return {
            notas: [],
            nota: {nombre: '', descripcion: ''}
        }
    },

    methods: {
        agregar() {
            //if (this.nota.nombre.trim() === '' || this.nota.descripcion.trim() === '') {
               // alert('Debes completar todos los campos antes de guardar');
               // return;
            //}

            const params = {
                nombre = this.nota.nombre,
                descripcion = this.nota.descripcion
            }

            this.nota.nombre = '';
            this.nota.descripcion = '';
            
            axios.post('/notas', params).then(res => {
                this.notas.push(res.data);
            })
        },

        eliminarNota(item, index) {
            axios.delete(`/notas/${item.id}`).then(() => {
                this.notas.splice(index, 1);
            })
        }
    }
}
</script>