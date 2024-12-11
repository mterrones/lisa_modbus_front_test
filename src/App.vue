<template>
  <v-app>
    <v-container>
      <v-card class="mt-5" outlined>
        <v-card-title class="headline text-center">
          Lisa MODBUS
        </v-card-title>
        <v-card-text>
          <v-data-table
            :headers="headers"
            :items="data"
            class="elevation-1"
            hide-default-footer
          >
            <template v-slot:top>
              <v-toolbar flat>
                <v-toolbar-title>La lectura de los datos:</v-toolbar-title>
                <v-spacer></v-spacer>
              </v-toolbar>
            </template>
          </v-data-table>
        </v-card-text>
      </v-card>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      headers: [
        { text: "Variable", value: "key" },
        { text: "Valor", value: "value" },
      ],
      data: [], // Datos iniciales vacíos
    };
  },
  mounted() {
    // Llamar a la función para cargar datos inicialmente
    this.loadData();

    // Establecer un intervalo para actualizar los datos cada 1 segundo
    this.interval = setInterval(this.loadData, 1000);
  },
  beforeDestroy() {
    // Limpiar el intervalo para evitar fugas de memoria
    clearInterval(this.interval);
  },
  methods: {
    // Función para cargar datos desde el servidor
    loadData() {
      fetch("http://localhost:4000/data")
        .then((response) => response.json())
        .then((json) => {
          this.data = Object.entries(json).map(([key, value]) => ({
            key,
            value,
          }));
        })
        .catch((error) => console.error("Error al cargar los datos:", error));
    },
  },
};
</script>

<style>
.text-center {
  text-align: center;
}
</style>