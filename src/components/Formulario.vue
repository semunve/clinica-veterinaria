<script setup>
import { reactive, computed } from "vue";
import Alerta from "./Alerta.vue";

const alerta = reactive({
  tipo: "",
  mensaje: "",
});
const emit = defineEmits([
  "update:nombre",
  "update:propietario",
  "update:email",
  "update:alta",
  "update:sintomas",
  "guardar-paciente",
]);

const props = defineProps({
  id: {
    type: [String, null],
    required: true,
  },
  nombre: {
    type: String,
    required: true,
  },
  propietario: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
  },
  alta: {
    type: String,
    required: true,
  },
  sintomas: {
    type: String,
    required: true,
  },
});
const validar = () => {
  if (Object.values(props).includes("")) {
    //Si alguno esta vacío
    alerta.mensaje = "Todos los campos son obligatorios";
    alerta.tipo = "error";
    console.log("si estan vacios");
    return;
  }
  emit("guardar-paciente");
  alerta.mensaje = "Paciente guardado correctamente";
  alerta.tipo = "exito";

  setTimeout(() => {
    Object.assign(alerta, {
      tipo: "",
      mensaje: "",
    });
  }, 3000);
};

const editando = computed(() => {
  return props.id;
});
</script>
<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-center">Seguimiento pacientes</h2>
    <p class="text-lg mt-5 text-center mb-10">
      Añade pacientes y
      <span class="text-indigo-600 font-bold">administralos</span>
    </p>

    <Alerta :alerta="alerta" v-if="alerta.mensaje" />
    <form
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
      @submit.prevent="validar"
    >
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold"
          >Nombre Mascota</label
        >
        <input
          name=""
          id="mascota"
          type="text"
          placeholder="Nombre de la mascota"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="nombre"
          @input="$emit('update:nombre', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="propietario" class="block text-gray-700 uppercase font-bold"
          >Nombre propietario</label
        >
        <input
          name=""
          id="propietario"
          type="text"
          placeholder="Nombre del propietario"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="propietario"
          @input="$emit('update:propietario', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold"
          >Email</label
        >
        <input
          name=""
          id="email"
          type="email"
          placeholder="Email de contacto"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="alta" class="block text-gray-700 uppercase font-bold"
          >Fecha de alta</label
        >
        <input
          name=""
          id="alta"
          type="date"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="alta"
          @input="$emit('update:alta', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold"
          >Sintomas
        </label>
        <textarea
          name=""
          id="sintomas"
          placeholder="Sintomas de la mascota"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="sintomas"
          @input="$emit('update:sintomas', $event.target.value)"
        />
      </div>
      <input
        type="submit"
        class="bg-indigo-600 w-full p-3 text-white font-bold hover:bg-indigo-400 cursor-pointer transition-colors"
        :value="[editando ? 'Guardar cambios' : 'Registrar paciente']"
      />
    </form>
  </div>
</template>
