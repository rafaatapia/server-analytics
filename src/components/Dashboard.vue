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
            <v-progress-linear v-if="isLoading" v-slot:progress color="blue" indeterminate></v-progress-linear>
            <v-data-table
              :headers="headers"
              :items="infos"
              :pagination.sync="pagination"
				      :rows-per-page-items="pagination.rowsPerPageItems"
              :search="search">              
              <template v-slot:items="props">
                <td class="text-xs-left">{{ props.item.memoria_total }}Gb</td>
                <td class="text-xs-left">{{ props.item.memoria_usada }}Gb</td>
                <td class="text-xs-left" :style="limiteMemoria(props.item)">{{ props.item.memoria_livre }}Gb</td>
                <td class="text-xs-left" :style="limiteCpu(props.item)">{{ props.item.cpu_usado }}%</td>
                <td class="text-xs-left">{{ props.item.data_registro | formatDate }}</td>                
                <td class="text-xs-right">
                  <v-btn @click="visualizar(props.item)" flat icon>
                    <v-icon>storage</v-icon>
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
      pagination: {
          sortBy: 'data_registro',
          descending: true,
					page: 1,
					rowsPerPage: 15,
					rowsPerPageItems: [15, 50, 100, {
						value: -1,
						text: 'Todos'
					}]
      },
      infos: [],
      search: '',
      isLoading: 'true',
      headers: [
          {
            text: 'Memoria Total',
            align: 'left',
            sortable: false,
            value: 'memoria_total'
          },
          {
            text: 'Memoria Usada',
            align: 'left',
            sortable: false,
            value: 'memoria_usada'
          },
          {
            text: 'Memoria Livre',
            align: 'left',
            sortable: false,
            value: 'memoria_livre'
          },
          {
            text: 'CPU Usado',
            align: 'left',
            sortable: false,
            value: 'cpu_usado'
          },
          {
            text: 'Data atualização',
            align: 'left',
            sortable: true,
            value: 'data_registro'
          },
          { 
            text: 'Discos', 
            value: 'iron',
            align: 'right'
          }
      ],
    }),
    mounted() {
      this.buscarEmpresas();
     
    },
    filters: {
      formatDate(value){
        if(value) {
          let data = new Date(value);
          let dia  = data.getDate().toString();
          let diaF = (dia.length == 1) ? '0'+dia : dia;
          let mes  = (data.getMonth()+1).toString(); //+1 pois no getMonth Janeiro começa com zero.
          let mesF = (mes.length == 1) ? '0'+mes : mes;
          let anoF = data.getFullYear();
          let hora = data.getHours().toString();
          let horaF = (hora.length == 1) ? '0'+hora : hora;
          let minuto = data.getMinutes().toString();
          let minutoF = (minuto.length == 1) ? '0'+minuto : minuto;
          let segundos = data.getSeconds().toString();
          let segundosF = (segundos.length == 1) ? '0'+segundos : segundos;

          return `${diaF}/${mesF}/${anoF} ${horaF}:${minutoF}:${segundosF}`;
        }
      }
    },
    methods: {
      visualizar(item) {        
        alert("Abra o console!");                
        console.log(item.discos[0]);
      },
      limiteCpu(item) {
        if(item.cpu_usado >= 80) {
          return "color: red; font-weight: bold";
        }        
      },
      limiteMemoria(item) {
        if(item.memoria_livre <= 0.500 ) {
          return "color: red; font-weight: bold";
        }
      },
      buscarEmpresas() {
         this.axios.get('http://ceb99c46.ngrok.io/monitorings?empresa=' + this.$route.params.empresa)
          .then( res => {
              this.isLoading = true;
              this.infos = res.data[0];                          
            }).finally( () => {
              this.isLoading = false;
            })
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
