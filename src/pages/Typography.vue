<template>
  <div v-if="showForm" class="form-container">
    <form @submit.prevent="guardarEquipo">
      <div class="form-group">
        <label for="id_categoria">Categoría</label>
        <select v-model="formData.id_categoria" id="id_categoria" required>
          <option v-for="categoria in categorias" :value="categoria.id" :key="categoria.id">
            {{ categoria.nombre }}
          </option>
        </select>
      </div>

      <div class="form-group">
        <label for="id_marca">Marca</label>
        <select v-model="formData.id_marca" id="id_marca" required>
          <option v-for="marca in marcas" :value="marca.id" :key="marca.id">
            {{ marca.nombre }}
          </option>
        </select>
      </div>

      <div class="form-group">
        <label for="id_modelo">Modelo</label>
        <select v-model="formData.id_modelo" id="id_modelo" required>
          <option v-for="modelo in modelos" :value="modelo.id" :key="modelo.id">
            {{ modelo.nombre }}
          </option>
        </select>
      </div>

      <div class="form-group">
        <label for="numero_serie">Número de Serie</label>
        <input type="text" v-model="formData.numero_serie" id="numero_serie" required />
      </div>

      <div class="form-group">
        <label for="id_so">Sistema Operativo</label>
        <select v-model="formData.id_so" id="id_so" required>
          <option v-for="so in sistemasOperativos" :value="so.id" :key="so.id">
            {{ so.nombre }}
          </option>
        </select>
      </div>
      <div class="form-group">
        <label for="id_licso">Licencia Sistema Operativo</label>
        <select v-model="formData.id_licso" id="id_licso" required>
          <option v-for="licso in licSistemaO" :value="licso.id" :key="licso.id">
            {{ licso.nombre }}
          </option>
        </select>
      </div>
      <div class="form-group">
        <label for="id_cpu">Procesador</label>
        <select v-model="formData.id_cpu" id="id_cpu" required>
          <option v-for=" cpu in procesador" :value="cpu.id" :key="cpu.id">
            {{ cpu.nombre }}
          </option>
        </select>
      </div>
      <div class="form-group">
        <label for="id_dd">Disco Duro</label>
        <select v-model="formData.id_dd" id="id_dd" required>
          <option v-for=" disco in discoDuro" :value="disco.id" :key="disco.id">
            {{ disco.nombre }}
          </option>
        </select>
      </div>
      <div class="form-group">
        <label for="id_tipo_dd">Tipo de Disco</label>
        <select v-model="formData.id_tipo_dd" id="id_tipo_dd" required>
          <option value="HDD">HDD</option>
          <option value="SSD">SSD</option>
        </select>
      </div>
      <div class="form-group">
        <label for="id_ram">Memoria RAM</label>
        <select v-model="formData.id_ram" id="id_ram" required>
          <option v-for=" ram in memoriaRam" :value="ram.id" :key="ram.id">
            {{ ram.nombre }}
          </option>
        </select>
      </div>
      <div class="form-group">
        <label for="id_ram">RAM</label>
        <input type="text" v-model="formData.id_ram" id="id_ram" required />
      </div>

      <div class="form-group">
        <label for="id_antivirus">Antivirus</label>
        <input type="text" v-model="formData.id_antivirus" id="id_antivirus" required />
      </div>

      <div class="form-group">
        <label for="id_office">Office</label>
        <input type="text" v-model="formData.id_office" id="id_office" required />
      </div>

      <div class="form-group">
        <label for="id_licoffice">Licencia de Office</label>
        <input type="text" v-model="formData.id_licoffice" id="id_licoffice" required />
      </div>

      <div class="form-group">
        <label for="fecha_compra">Fecha de Compra</label>
        <input type="date" v-model="formData.fecha_compra" id="fecha_compra" required />
      </div>

      <div class="form-group">
        <label for="costo">Costo</label>
        <input type="number" v-model="formData.costo" id="costo" required />
      </div>

      <button type="submit">{{ editMode ? 'Actualizar Equipo' : 'Agregar Equipo' }}</button>
      <button type="button" @click="cancelForm">Cancelar</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      showForm: false,
      editMode: false,
      equipos: [],
      categorias: [],
      marcas: [],
      modelos: [],
      sistemasOperativos: [],
      formData: {
        id_equipo: null,
        id_categoria: "",
        id_marca: "",
        id_modelo: "",
        numero_serie: "",
        id_so: "",
        id_licso: "",
        id_cpu: "",
        id_dd: "",
        id_tipo_dd: "",
        id_ram: "",
        id_antivirus: "",
        id_office: "",
        id_licoffice: "",
        fecha_compra: "",
        costo: "",
      },
    };
  },
  mounted() {
    this.fetchCategorias();
    this.fetchMarcas();
    this.fetchModelos();
    this.fetchSistemasOperativos();
  },
  methods: {
    async fetchCategorias() {
      try {
        const response = await axios.get('/api/categorias');
        this.categorias = response.data;
      } catch (error) {
        console.error("Error al obtener las categorías:", error);
      }
    },
    async fetchMarcas() {
      try {
        const response = await axios.get('/api/marcas');
        this.marcas = response.data;
      } catch (error) {
        console.error("Error al obtener las marcas:", error);
      }
    },
    async fetchModelos() {
      try {
        const response = await axios.get('/api/modelos');
        this.modelos = response.data;
      } catch (error) {
        console.error("Error al obtener los modelos:", error);
      }
    },
    async fetchSistemasOperativos() {
      try {
        const response = await axios.get('/api/sistemas-operativos');
        this.sistemasOperativos = response.data;
      } catch (error) {
        console.error("Error al obtener los sistemas operativos:", error);
      }
    },
    async guardarEquipo() {
      try {
        if (this.editMode) {
          await axios.put(`/api/equipos/${this.formData.id_equipo}`, this.formData);
        } else {
          await axios.post('/api/equipos', this.formData);
        }
        this.fetchEquipos();
        this.cancelForm();
      } catch (error) {
        console.error("Error al guardar el equipo:", error);
      }
    },
    cancelForm() {
      this.showForm = false;
      this.resetForm();
    },
    resetForm() {
      this.formData = {
        id_equipo: null,
        id_categoria: "",
        id_marca: "",
        id_modelo: "",
        numero_serie: "",
        id_so: "",
        id_licso: "",
        id_cpu: "",
        id_dd: "",
        id_tipo_dd: "",
        id_ram: "",
        id_antivirus: "",
        id_office: "",
        id_licoffice: "",
        fecha_compra: "",
        costo: "",
      };
    },
  },
};
</script>
