<template>
  <div
    v-for="value in ListaDwitterCalculate().reverse()"
    :key="value.text"
    style="border: 0.5px solid #afafaf; padding: 20px 20px 0px 20px"
  >
    <div class="flex flex-row">
      <div class="basis-1/12">
        <img v-bind:src="value.image" style="width: 20px; border-radius: 20px" />
      </div>
      <div class="basis-11/12">{{ value.owner }} | {{ value.datatime }}</div>
    </div>
    <div class="flex flex-row">
      <div class="basis-full" style="text-align: center">
        <span style>{{ value.text }}</span>
      </div>
    </div>
    <div>
      <!-- -->
 
    </div>
  </div>
</template>
<script>
import makeBlockie from "ethereum-blockies-base64";

import Config from "../stores/config.js";


export default {
  props: {
  Dwitters: Object},
  data() {
    return {
   
      addressConnection: "",
  
      fee: "", modalshow: false,
      messaggioModal: 'prova messaggio',
      linkShare: '', textCopied: '',
    }
  },
  mounted() {
    this.linkShare = Config.linkShare;

  },
  methods: {
    ListaDwitterCalculate() {
      return this.Dwitters.map((item, index) => {
        var data = new Date(item.timestamp * 1000);

        return {
          image: makeBlockie(item.owner),
          owner: item.owner,
          datatime:
            data.getMonth() +
            1 +
            "/" +
            data.getDay() +
            "/" +
            data.getFullYear() +
            " " +
            data.getHours() +
            ":" +
            data.getMinutes() +
            ":" +
            data.getSeconds(),
          text: item.text,
          id: index,
        };
      });
    }, 
    copytext(textocpy) {
      const el = document.createElement('textarea');  
        el.value =textocpy;                                 
        el.setAttribute('readonly', '');                
        el.style.position = 'absolute';                     
        el.style.left = '-9999px';                      
        document.body.appendChild(el);                  
        const selected =  document.getSelection().rangeCount > 0  ? document.getSelection().getRangeAt(0) : false;                                    
        el.select();                                    
        document.execCommand('copy'); 
        
        
   /*   navigator.clipboard.writeText(textocpy);*/
/*this.$refs.myinput.focus();
      document.execCommand('copy'); */
      this.textCopied = "Copied - " + textocpy;
      setTimeout(() => this.textCopied = '', 3000)
    },
    shareMobile(shareData) { navigator.share(shareData) },
  }
}
</script>





