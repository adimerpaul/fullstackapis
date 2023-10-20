<template>
  <q-page class="q-pa-md">
    <q-form @submit="mascotasSave">
      <div class="row">
        <div class="col-4">
          <q-input label="nombre" v-model="mascota.nombre" required />
        </div>
        <div class="col-4">
          <q-select
            label="tipo"
            v-model="mascota.tipo"
            :options="['GATO', 'PERROS']"
            required
          />
        </div>
        <div class="col-4">
          <q-input label="color" v-model="mascota.color" required />
        </div>
        <div class="col-4">
          <q-input
            label="celular"
            type="number"
            v-model="mascota.celular"
            required
          />
        </div>
        <div class="col-4">
          <q-input label="fechaNac" type="date" v-model="mascota.fechaNac" />
        </div>
        <div class="col-4 flex flex-center">
          <q-btn label="guardar" color="green" type="submit" />
        </div>
      </div>
    </q-form>
    <div class="text-bold text-h3">{{ msg }}</div>
    <table style="width: 100%">
      <tr>
        <th>#</th>
        <th>Nombre</th>
        <th>Tipo</th>
        <th>Color</th>
        <th>Celular</th>
        <th>Fecha Nac</th>
        <th>Opciones</th>
      </tr>
      <tr v-for="m in mascotas" :key="m.id">
        <td>{{ m.id }}</td>
        <td>{{ m.nombre }}</td>
        <td>{{ m.tipo }}</td>
        <td>{{ m.color }}</td>
        <td>{{ m.celular }}</td>
        <td>{{ m.fechaNac }}</td>
        <td>
          <q-btn
            icon="delete"
            color="red"
            dense
            size="10px"
            @click="eliminar(m)"
          />
          <q-btn
            icon="edit"
            color="orange"
            dense
            size="10px"
            @click="modificar(m)"
          />
        </td>
      </tr>
    </table>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "IndexPage",
  data() {
    return {
      msg: "Datos de mascotas",
      mascotas: [],
      mascota: {
        fechaNac: "2023-10-19",
      },
    };
  },
  created() {
    this.mascotasGet();
  },
  methods: {
    mascotasGet() {
      this.$axios.get("http://localhost:8000/api/mascotas").then((res) => {
        this.mascotas = res.data;
      });
    },
    mascotasSave() {
      if (this.mascota.id == undefined) {
        this.$axios
          .post("http://localhost:8000/api/mascotas", this.mascota)
          .then((res) => {
            this.mascota = { fechaNac: "2023-10-19" };
            this.mascotasGet();
          });
      } else {
        this.$axios
          .put(
            "http://localhost:8000/api/mascotas/" + this.mascota.id,
            this.mascota
          )
          .then((res) => {
            this.mascota = { fechaNac: "2023-10-19" };
            this.mascotasGet();
          });
      }
    },
    eliminar(mascota) {
      this.$axios
        .delete("http://localhost:8000/api/mascotas/" + mascota.id)
        .then((res) => {
          this.mascotasGet();
        });
    },
    modificar(mascota) {
      this.mascota = mascota;
    },
  },
});
</script>
