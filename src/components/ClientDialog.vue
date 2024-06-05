<style lang="scss">
</style>

<template>
  <v-card>
    <v-card-title><h3>{{ client.givenName || '' }}</h3></v-card-title>
    <v-card-text>
      <div>
        <v-row no-gutters>
          <v-col sm="4"><v-text-field v-model="client.email" :label="'Email'" class="ma-2" readonly /></v-col>
          <v-col sm="4"><v-text-field v-model="client.familyName1" :label="'Familia'" class="ma-2" readonly /></v-col>
          <v-col sm="4"><v-text-field v-model="client.phone" :label="'Telèfon'" class="ma-2" readonly /></v-col>
        </v-row>
        <v-row no-gutters>
          <v-col><v-text-field v-model="client.docType" :label="'Tipus de document'" class="ma-2" readonly /></v-col>
          <v-col><v-text-field v-model="client.docNum" :label="'Número de document'" class="ma-2" readonly /></v-col>
        </v-row>
      </div>
      <div>
        <h2 class="mt-5 mb-2 ml-2">Productes</h2>
        <v-data-table :headers="headers" :items="contractedProducts" :items-per-page="10" :footer-props="{'items-per-page-text':'Productes per pàgina'}">
          <template v-slot:item.contract="{item}">
            <span v-if="item.mbSpeed">{{ item.mbSpeed }} mb/s</span>
            <span v-else-if="item.gbData">{{ item.gbData }} GB</span>
          </template>
          <template v-slot:item.soldAt="{item}">
            <span>{{ moment(item.soldAt).locale('ca').format('LLL') }}</span>
          </template>
        </v-data-table>
      </div>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props:{
    customerId:{type:String,default:null}
  },
  data: () => ({
    client:{},
    headers: [
      {text:'Nom',value:'productName'},
      {text:'Numeració',value:'numeracioTerminal'},
      {text:'Contracte',value:'contract'},
      {text:'Tipus',value:'productTypeName'},
      {text:'Venut',value:'soldAt'},
    ],
  }),
  watch:{
    async customerId(){
      await this.downloadClientDatas();
    }
  },
  computed:{
    contractedProducts(){
      if(!this.client||!Object.keys(this.client).length)return [];
      return this.client.products;
    },
  },
  async mounted(){
    await this.downloadClientDatas();
  },
  methods: {
    async downloadClientDatas(){
      const client = (await fetch('https://mockapi-json.vercel.app/clients?customerId='+this.customerId).then(response => response.json()).then(client => client))[0];
      const products = await fetch('https://mockapi-json.vercel.app/products?customerId='+this.customerId).then(response => response.json()).then(client => client);

      this.client = {...client,products:[...products]};
    }
  },
}
</script>