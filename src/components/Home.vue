<style lang="scss">
</style>

<template>
  <div class="ma-3">
    <div class="filters d-flex flex-col">
      <v-select v-model="sFamilies" class="mr-3" :items="aFamilies" label="Families" multiple clearable />
      <v-text-field v-model="search" label="Cercar..." style="max-width: 30vw;"/>
    </div>
    <v-data-table :headers="headers" :items="filteredClients" :items-per-page="13" :search="search" class="elevation-2" :footer-props="{'items-per-page-text':'Clients per pàgina'}">
      <template v-slot:item.actions="{item}">
        <v-btn icon @click="showClient(item.customerId)"><v-icon>mdi-eye</v-icon></v-btn>
      </template>
    </v-data-table>
    <v-dialog v-if="clientDialog.show" v-model="clientDialog.show" width="45vw">
      <client-dialog-edit :customerId="clientDialog.customerId" />
    </v-dialog>
  </div>
</template>

<script>
import ClientDialogEdit from './ClientDialog.vue';
export default {
  components:{ClientDialogEdit},
  props:{
  },
  data: () => ({
    headers: [
      {text:'Nom',value:'givenName'},
      {text:'Familia',value:'familyName1'},
      {text:'Email',value:'email'},
      {text:'',value:'actions',width:10}
    ],
    
    search:'',

    clients:[],
    clientDialog:{show:false,customerId:null},

    aFamilies:[],
    sFamilies:[],
  }),
  computed:{
    filteredClients(){
      if(!this.clients||!this.clients.length)return [];
      let clients = this.clients;
      if(this.sFamilies&&this.sFamilies.length)clients = clients.filter(x => this.sFamilies.includes(x.familyName1));
      return clients;
    },
  },  
  async mounted(){
    await this.downloadDatas();
  },
  methods:{
    async downloadDatas(){
      const clients = await fetch('https://mockapi-test-tecnic.onrender.com/clients').then(response => response.json())
      .then(clients => clients);
      this.clients = clients;

      this.aFamilies = [...new Set(this.clients.map(client => client.familyName1))].map(family => ({text:family,value:family}));
    },
    showClient(customerId){
      if(!customerId)return;
      this.clientDialog = {show:true,customerId:customerId};
    },
  },  
}
</script>