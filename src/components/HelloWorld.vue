<template>
  <v-container>
    <v-layout
      text-xs-center
      wrap>
      <v-flex xs12>
        <v-img
          :src="require('../assets/logo_transparent.png')"
          contain
          class="imagem"
          height="300"
        ></v-img>
      </v-flex>

      <v-flex
        mb-5
        xs12>

        <h2 class="headline font-weight-bold mb-3">Monitoramento</h2>         

        <v-layout justify-center>
          <v-card>
            <v-card-title>                            
              <v-text-field
                v-model="search"
                append-icon="search"
                label="Search"
                single-line
                hide-details
              ></v-text-field>
            </v-card-title>
            <v-data-table
              :headers="headers"
              :items="empresas"
              :search="search">
              <template v-slot:items="props">
                <td class="text-xs-left">{{ props.item.empresa }}</td>                
                <td class="text-xs-right">
                  <v-btn @click="visualizar(props.item.empresa)" flat icon>
                    <v-icon>remove_red_eye</v-icon>
                  </v-btn>
                </td>
              </template>
              <v-alert v-slot:no-results :value="true" color="error" icon="warning">
               Não encontramos nenhuma empresa com o nome "{{ search }}".               
              </v-alert>
            </v-data-table>
          </v-card>
        </v-layout>                 
      </v-flex>


    
    </v-layout>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
      empresas: [],
      search: '',
      headers: [
          {
            text: 'Empresa',
            align: 'left',
            sortable: true,
            value: 'empresa'
          },    
          { 
            text: 'Visualizar', 
            value: 'iron',
            align: 'right'
          }
      ],
    }),
    mounted() {
      this.buscarEmpresas();
     
    },
    methods: {
      visualizar(nome) {        
        this.$router.push({name: 'Dashboard', params: {empresa: nome.toLowerCase()}});
      },
      buscarEmpresas() {
         this.axios.get('http://ceb99c46.ngrok.io/monitorings')
          .then( res => {
            let nomeEmpresas = [... new Set(res.data.map( x => x.empresa))];            
            for(let i = 0; i < nomeEmpresas.length; i++) {              
              this.empresas.push({empresa: nomeEmpresas[i].charAt(0).toUpperCase() + nomeEmpresas[i].slice(1)})
            }
          });
      }
    }    
  }
</script>

<style>
 .imagem  {
   margin-top: -60px;
   margin-bottom: -65px;
 }
</style>
