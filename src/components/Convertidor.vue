<template>
  <div>
    <q-select  v-model.number="tasa" :options="opciones" emit-value map-options />
    <q-input 
      name="usd"
      filled 
      v-model="usd" 
      @keyup="cambiaVEF"
      @focus="enfoca($event)"
      @blur="desenfoca($event)"
      prefix="$" 
      input-class="text-right sin-flechas"
    />
    <br>
    <p class="text-center"><b>Equivalen a</b></p>

    <q-input 
      name="bs"
      filled 
      v-model.number="bs" 
      @keyup="cambiaUSD"
      @focus="enfoca($event)"
      @blur="desenfoca($event)"
      prefix="Bs" 
      input-class="text-right sin-flechas"/>
    <br>
  </div>
</template>

<script>
const formatter = new Intl.NumberFormat("es-VE", {
  style: "decimal",
  minimumFractionDigits: 0,
});

export default {
  name: "Convertidor",
  props: ['tasas'],
  data(){
    return {
      usd: null,
      bs: null,
      tasa: this.tasas.enparalelo.valor,
      opciones: [
        {
          label: 'Monitor dolar',
          value: this.tasas.enparalelo.valor
        },
        {
          label: 'Dolar Today',
          value: this.tasas.dolartoday.valor
        },
        {
          label: 'BCV',
          value: this.tasas.bcv.valor
        },
        {
          label: 'Yadio',
          value: this.tasas.yadio.valor
        },
        {
          label: 'Airtm',
          value: this.tasas.airtm.valor
        }
      ]
    }
  },
  methods: {
    cambiaVEF() {
      this.bs = formatter.format((this.usd * this.tasa).toFixed());
    },

    cambiaUSD(){
      this.usd = formatter.format((this.bs / this.tasa).toFixed(2));
    },

    enfoca(event){
      if(event.target.name == 'usd' && this.usd != null ) {
        this.usd = parseFloat(this.usd.split(',').join('.'))
        event.target.type = 'number'
        this.cambiaVEF()
      } else if(event.target.name == 'bs' && this.bs != null) {
        this.bs = parseFloat(this.bs.split('.').join(''))
        event.target.type = 'number'
        this.cambiaUSD()
      }
    },
    desenfoca(evento){
      evento.target.type = "type"
    }
  }
}
</script>

