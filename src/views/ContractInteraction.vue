<template>
  <div class="sm:container sm:mx-auto" style="max-width: 650px">
 
    <h2>Welcome to the Decentralized MicroBlog</h2>
    in this page can see how to interact withi contract<br>
    now with provider default can read contract.
    <div>
      <ClistDwitter :Dwitters="Dwitters" />
    </div>
   <!-- <button v-on:click="trasferimentoFondi()">Trasferisci Fondi</button>-->
  </div>
</template>
<script>

import Dwitter from "../contracts/Dwitter.json";
import Config from "../stores/config.js";
import { ethers } from "ethers";
import ClistDwitter from '../components/listDwitter.vue'


export default {

  components: {  ClistDwitter },
  data() {
    return {
      Dwitters: [],
    

    };
  },
  mounted() {
  
    const provider = new ethers.getDefaultProvider(Config.irpDefault);
    let Contract = new ethers.Contract(
      Config.addressContractDwitter,
      Dwitter.abi,
      provider
    );

    Contract.getDwitters().then((Response) => {
      this.Dwitters = Response;

    });


}
}


</script>



<style>
.btn {
  @apply py-1 px-2 rounded;
}

.btn-blue {
  @apply bg-teal-500 text-white border border-blue-500;
}
.btn-blue:hover {
  @apply bg-transparent text-teal-500;
}
</style>