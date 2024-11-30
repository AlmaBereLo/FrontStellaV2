<template>
  <div class="container mt-4">
    <h2 class="typo-line mb-4">Gestión de Agencias</h2>
    <div class="text-right mb-4">
      <button class="btn btn-primary" @click="openForm">
        <i class="fa-solid fa-plus"></i> Agregar
      </button>
    </div>
    <div v-if="showForm" class="modal-overlay" @click.self="cancelForm">
      <div class="card modal-content">
        <div class="card-header">
          <h4 class="mb-4">{{ editMode ? 'Editar Agencia' : 'Agregar Agencia' }}</h4>
        </div>
        <div class="card-body">
          <form @submit.prevent="guardarAgencia">
            <div class="form-group">
              <label for="nombre_age">Nombre Agencia</label>
              <input 
                type="text"
                id="nombre_age"
                v-model="formData.nombre_age"
                class="form-control"
                placeholder="Nombre de la agencia"
                required
              />
            </div>
            <div class="text-center">
              <button type="submit" class="btn btn-success mr-2">
                <i class="fa-solid fa-check"></i> Guardar
              </button>
              <button type="button" class="btn btn-secondary" @click="cancelForm">
                <i class="fa-solid fa-times"></i>Cancelar
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
    <div class="table-responsive">
      <table class="table table-striped table-hover text-center">
        <thead class="thead-dark">
          <tr>
            <th scope="col">#</th>
            <th scope="col">Nombre</th>
            <th scope="col">Acciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="agencia in agencias" :key="agencia.id_agencia">
            <td>{{ agencia.id_agencia }}</td>
            <td>{{ agencia.nombre_age }}</td>
            <td class="td-actions">
              <button class="btn btn-warning btn-sm" @click="editAgency(agencia)">
                <i class="fa-solid fa-pen-to-square"></i>
              </button>
              <button class="btn btn-danger btn-sm" @click="eliminarAgencia(agencia.id_agencia)">
                <i class="fa-solid fa-trash"></i>
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import axios from "axios";
const apiUrl = "https://d854-189-164-39-38.ngrok-free.app/api";
export default {
  data() {
    return {
      showForm: false,
      editMode: false,
      agencias: [],
      formData: {
        id_agencia: null,
        nombre_age: "",
      },
    };
  },
  mounted() {
    this.fetchAgencias();
  },
  methods: {
    async fetchAgencias() {
      try {
        const response = await axios.get(`${apiUrl}/agencia`, {
          headers: {
            "ngrok-skip-browser-warning": "true",
          },
        });
        console.log("Datos obtenidos:", response.data);
        if (Array.isArray(response.data)) {
          this.agencias = response.data;
        } else {
          console.error("Datos no válidos recibidos de la API");
        }
      } catch (error) {
        console.error("Error al obtener los datos de la API:", error);
      }
    },
    async guardarAgencia() {
      try {
        if (this.formData.id_agencia) {
          // Actualizar agencia existente
          await axios.put(`${apiUrl}/agencia/${this.formData.id_agencia}`, this.formData);
        } else {
          // Crear nueva agencia
          await axios.post(`${apiUrl}/agencia`, this.formData); // Usar backticks aquí
        }
        this.fetchAgencias();
        this.cancelForm();
      } catch (error) {
        console.error("Error al guardar la agencia:", error);
      }
    },
    // Eliminar una agencia
    async eliminarAgencia(id) {
      if (confirm("¿Estás seguro de eliminar esta agencia?")) {
        try {
          await axios.delete(`${apiUrl}/agencia/${id}`);
          this.fetchAgencias();
        } catch (error) {
          console.error("Error al eliminar la agencia:", error);
        }
      }
    },
    // Mostrar el formulario para agregar o editar
    openForm() {
      this.showForm = true;
      this.editMode = false;
      this.resetForm();
    },
    // Cancelar el formulario
    cancelForm() {
      this.showForm = false;
      this.resetForm();
    },
    // Resetear los datos del formulario
    resetForm() {
      this.formData = {
        id_agencia: null,
        nombre_age: "",
      };
    },
    // Editar una agencia
    editAgency(agency) {
      this.formData = { ...agency }; // Asignar los datos de la agencia al formulario
      this.showForm = true;
      this.editMode = true;
    },
  },
};
</script>
