<script setup>
  import {ref, watch } from 'vue';
  import Header from './components/Header.vue';
  import Buttons from './components/Buttons.vue';
  import {calcularTotalPagar} from './helpers';

  const cantidad = ref(10000);
  const meses = ref(6);
  const total = ref(0);

  const MIN = 0;
  const MAX = 20000;
  const STEP = 100;

  const formatearDinero = (valor) => {
    const formatter = new Intl.NumberFormat('en-US', {
      style: 'currency',
      currency: 'EUR'
    });

    return formatter.format(valor);
  };

  watch([cantidad, meses], () => {
    total.value = calcularTotalPagar(cantidad.value, meses.value);
  }, {inmediate: true});

  const handleChangeDecremento = () => {

    const valor = cantidad.value - STEP;
    
    if(valor < MIN){
      alert('Cantidad no válida');
      return;
    }
    cantidad.value = valor;
  }

  const handleChangeIncremento = () => {

  const valor = cantidad.value + STEP;

      if(valor > MAX){
        alert('Cantidad no válida');
        return;
      }
      cantidad.value = valor;
  }

</script>

<template>
  <div class="my-20 max-w-lg mx-auto bg-white shadow p-10">
    <Header />
    <div class="flex justify-between mt-10">
      <Buttons 
        :operador="'-'"
        @fn="handleChangeDecremento"
      />
      <Buttons 
        :operador="'+'"
        @fn="handleChangeIncremento"
      />
    </div>
    <div class="my-5">
      <input 
        type="range"
        class="w-full bg-gray-200 accent-lime-500 hover:accent-lime-600"
        :min="MIN"
        :max="MAX"
        :step="STEP"
        v-model.number="cantidad"
      />
      <p v-text="formatearDinero(cantidad)" class="text-center my-10 text-5xl font-extrabold text-indigo-600"></p>
      <h2 class="text-2xl font-extrabold text-gray-500 text-center">
        Elige un <span class="text-indigo-600">Plazo </span>a pagar
      </h2>
      <select
        class="w-full p-2 bg-white border border-gray-300 rounded-lg text-center text-xl font-bold text-gray-500 mt-5"
        :value="meses"
        v-model.number="meses"
      >
        <option value="6">6 Meses</option>
        <option value="12">12 Meses</option>
        <option value="24">24 Meses</option>
      </select>
    </div>
    <div v-if="total > 0" class="my-5 space-y-3 bg-gray-200 p-5">
      <h2 class="text-2xl font-extrabold text-gray-700 text-center">
        Resumen <span class="text-indigo-600">de pagos</span>
      </h2>
      <p class="text-xl text-gray-700 text-center font-bold">Meses: {{ meses }}</p>
      <p class="text-xl text-gray-700 text-center font-bold">Total a pagar: {{ formatearDinero(total) }}</p>
      <p class="text-xl text-gray-700 text-center font-bold">Mensualidades: {{ formatearDinero(total / meses) }}</p>
    </div>

    <p v-else class="text-center"> Añade una cantidad y un plazo a pagar</p>
  </div>
</template>

