<template>

<div class="container">
  <div class="col-md-12">
    <!--Encabezado-->
      <EncabezadoApp />
    <!--fin Encabezado-->
    <!--Balance-->
      <BalanceApp :total="total" :numeroTransacciones="numeroTransacciones" />
    <!--fin Balance-->
    <!--Border-->
      <Border />
    <!--fin border-->
    <!--Resumen-->
      <ResumenApp :ingresos="ingresos" :gastos="gastos" />
    <!--/Resumen-->
    <!--HistorialTransacciones-->
      <HistorialTransacciones :transacciones="transacciones" @eliminar-transaccion="eliminarTransaccion($event)" />
    <!--/HistorialTransacciones-->
    <!--AgregarTransaccion-->
      <AgregaTransaccionApp @agregar-transaccion="agregarTransaccion" />
    <!--/AgregarTransaccion-->
  </div>
</div>

</template>

<script setup lang="ts">
import EncabezadoApp from './EncabezadoApp.vue'
import BalanceApp from './BalanceApp.vue'
import ResumenApp from './ResumenApp.vue';
import HistorialTransacciones from './HistorialTransacciones.vue';
import AgregaTransaccionApp from './AgregaTransaccionApp.vue';
import Border from './Border.vue';
import { ref, computed } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

type Transaccion = {
  id: number;
  nombre: string;
  monto: number;
}



//declarar un arreglo reactivo de tipo transaccion
const transacciones = ref<Transaccion[]>([
  {
    id: 1,
    nombre: 'Salario',
    monto: 2000000
  },
  {
    id: 2,
    nombre: 'Segunda transacción',
    monto: 600000
  },
  {
    id: 3,
    nombre: 'Tercera transacción',
    monto: -20000
  },
  {
    id: 4,
    nombre: 'Cuarta transacción',
    monto: -100000
  }
]);

//metodo para agregar una transaccion
const agregarTransaccion = (transaccion: Transaccion) => {
  transacciones.value.push(transaccion);
  toast.success('Transacción agregada correctamente');
};

//metodo para eliminar transaccion
const eliminarTransaccion = (id: number) => {
  transacciones.value = transacciones.value.filter(item => item.id !== id);
  toast.success('Transacción eliminada correctamente');
};

const total = computed(() => {
  return transacciones.value.reduce((acc, item) => acc + item.monto, 0);
});

const numeroTransacciones = computed(() => {
  return transacciones.value.length;
});

const ingresos = computed(() => {
  return transacciones.value.filter(item => item.monto > 0).reduce((acc, item) => acc + item.monto, 0);
});

const gastos = computed(() => {
  return transacciones.value.filter(item => item.monto < 0).reduce((acc, item) => acc + item.monto, 0);
});




</script>