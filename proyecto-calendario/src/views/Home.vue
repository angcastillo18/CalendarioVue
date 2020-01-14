<template>
  <div>
    <v-row >
      <v-col cols="12">
        <v-card>
          <v-date-picker v-model="fecha" 
          full-width
          locale="es-PE"  
          :min="minimo"
          :max="maximo"
          >

          </v-date-picker>
        </v-card>
        <v-card color="error" dark>
          <v-card-text class="display-1 text-center">
            Valor del dólar: {{valor}} - {{fecha}}
          </v-card-text>
        </v-card>
      </v-col>
      
    </v-row>
  </div>
</template>

<script>
//importar axios
import axios from "axios";


export default {
  name: 'home',
  components: {
  },
  data(){
    return{
      fecha:new Date().toISOString().substr(0, 10),
      minimo:'1984',
      maximo:new Date().toISOString().substr(0, 10),
      valor:null
    }
  },
  methods:{
    async getDolar(dia){
      
      let datos=await axios.get(`https://mindicador.cl/api/dolar/${dia}`)
      console.log(datos.data.serie[0].valor);
      this.valor=await datos.data.serie[0].valor;
    }
  },
  created() {
    this.getDolar('01-02-2019');
    
    //min 31.51 del video
  },
  
}
</script>
