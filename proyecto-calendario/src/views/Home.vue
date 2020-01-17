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
          @change="getDolar(fecha)"
          >

          </v-date-picker>
        </v-card>
        <v-card color="error" dark>
          <v-card-text class="display-1 text-center">
            Valor del dólar: {{valor}} - del Día: {{fecha}}
          </v-card-text>
        </v-card>
      </v-col>
      
    </v-row>
  </div>
</template>

<script>
//importar axios
import axios from "axios";
//importamos losmetodos de vuex
import {mapMutations} from "vuex";

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
    //llamamos a las mutaciones
    ...mapMutations(['mostrarLoading','ocultarLoading']),
    //
    async getDolar(dia){
      //tenemos que voltear el orden de la fecha captada en el v-model fecha
      //porque el api recibe la fecha alreves
      let newFormatDate=dia.split("-").reverse().join("-");
      //console.log(newFormatDate);
      //try catch para manejar errores
      try {
        //mostrar el loading
        this.mostrarLoading({titulo:'Accediendo a información',color:'secondary'})
        //
          
          let datos=await axios.get(`https://mindicador.cl/api/dolar/${newFormatDate}`)
          //console.log(datos.data.serie[0]);
          //los dias sabados y domingos no trae datos del api 
          //por eso que se hace la validacion del datos.data.serie[0]== vacio
          if(datos.data.serie[0]!== undefined){
            this.valor=await datos.data.serie[0].valor;  
          }else{
            this.valor='Sin resultados';
          }
      } catch (error) {
        console.log(error);
      }
      //finally se ejecuta luego del try catch, asi alla error o no
      finally{
        //cuando termina la carga
        this.ocultarLoading();
      }
       
    }
  },
  created() {
    this.getDolar(this.fecha);
    
    //min 31.51 del video
  },
  
}
</script>
