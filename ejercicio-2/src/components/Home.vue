<template>
    <div class="container-fluid mt-3" id="app">
        <input type="text" class="form-control" v-model="filtroNombre"
            placeholder="Buscar por nombre">
        <div class="alert alert-primary mt-1" role="alert" v-show="(filtroNombre.length > 0 && filtroNombre.length < 3)">
            Ingrese al menos 3 caracteres
        </div>
        <input type="text" class="form-control mt-3" v-model="filtroDni"
            placeholder="Buscar por dni">
        <div class="alert alert-light mt-1" role="alert" v-show="(filtroDni.length > 0 && filtroDni.length < 3)">
            Ingrese al menos 3 caracteres
        </div>
        <br>
        <div class="card-deck m-0">
            <div class="row">
                <div class="col" v-for="persona in personasFiltradas" :key="persona.dni">
                    <div class="card mb-3">
                        <div class="card-body">
                            <h5 class="card-title">{{getNombreCompleto(persona)}}</h5>
                            <p class="card-text">dni {{persona.dni}}</p>
                            <a href="#" class="card-link">{{persona.correo}}</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import { ref, computed, onMounted } from 'vue';

export default {
    name: "HomeView",
    setup() {
        const criterioDeBusqueda = ref('');
        const personas = ref([]);
        const filtroNombre = ref('')
        const filtroDni = ref('')

        onMounted(() => {
        personas.value = [
            {
            nombre: "Daniel",
            apellido: "Sanchez",
            correo: "danielsanchez68@hotmail.com",
            dni: "20442873"
            },
            {
            nombre: "Juan",
            apellido: "Perez",
            correo: "j@p.gmail.com",
            dni: "12345678"
            },
            {
            nombre: "Ana",
            apellido: "Suarez",
            correo: "a@s.gmail.com",
            dni: "87654321"
            },
            {
            nombre: "...",
            apellido: "...",
            correo: "...",
            dni: "..."
            },
        ];
        });

        const personasFiltradas = computed(() => {
        return personas.value.filter((persona) => {
            let cumpleConFiltros = true
            if(filtroNombre.value != '' && filtroNombre.value.length >= 3){
                cumpleConFiltros = getNombreCompleto(persona).toLowerCase().includes(filtroNombre.value)
            }
            if(filtroDni.value != '' && filtroDni.value.length >= 3){
                cumpleConFiltros = persona.dni.toLowerCase().includes(filtroDni.value)
            }
            return cumpleConFiltros
        });
        });

        const getNombreCompleto = (persona) => {
            return `${persona.nombre} ${persona.apellido}`;
        };

        return {
        criterioDeBusqueda,
        filtroNombre,
        filtroDni,
        personasFiltradas,
        getNombreCompleto
        };
    }
};
</script>