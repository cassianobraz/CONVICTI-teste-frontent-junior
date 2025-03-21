<template>
  <div class="p-6 pr-11 bg-white rounded-lg w-[916px] h-[342px]">
    <div class="flex justify-between items-center">
      <h2 class="text-xl font-semibold mb-2">Usuários</h2>
      <a class="cursor-pointer hover:bg-purple-circle2" @click="openModalForAdd"><img :src="imgPlus"
          alt="icon de adicionar" /></a>
    </div>
    <div class="border-t border-border-config"></div>
    <table class="w-full border-collapse">
      <thead>
        <tr class="border-b border-border-config text-left text-xs">
          <th class="py-2 text-font-text-title text-xs text-left w-[163px]">Nome</th>
          <th class="py-2 text-font-text-title text-xs text-left w-[270px]">Email</th>
          <th class="py-2 text-font-text-title text-xs text-left w-[220px]">Perfil</th>
          <th class="py-2 text-font-text-title text-xs text-left w-[80px]">Status</th>
          <th class="py-2 text-font-text-title text-xs w-[40px] text-right"></th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td colspan="2" class="h-2"></td>
        </tr>
        <tr v-for="(usuario, index) in usuarios" :key="index"
          :class="index % 2 === 0 ? 'bg-font-line h-[26px]' : 'bg-white h-[40px]'">
          <td class="px-2 h-[26px] rounded-l">
            <p class="text-sm font-medium">{{ usuario.nome }}</p>
          </td>
          <td class="h-[26px]">
            <p class="text-sm">{{ usuario.email }}</p>
          </td>
          <td class="h-[26px]">
            <span class="text-xs font-medium">{{ usuario.perfil }}</span>
          </td>
          <td class="h-[26px] text-center rounded-r">
            <span
              class="bg-border-ativo text-xs font-medium w-[84px] h-[20px] flex items-center justify-center uppercase text-font-ativo rounded">
              {{ usuario.status }}
            </span>
          </td>
          <td class="w-[14px] h-[14px] bg-white">
            <img :src="imgEdit" alt="icon de editar" class="ml-6 cursor-pointer" @click="openModalForEdit(usuario)" />
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Modal -->
    <div v-if="showModal" class="absolute insert-0 flex items-center justify-center z-50 -mt-[25.8rem] ml-[5.1rem]">
      <div class="bg-white rounded-lg shadow-lg px-8 w-[506px] h-[412px]">
        <h2 class="text-xl font-semibold py-4 mt-4 -mb-3">{{ isEditMode ? 'Editar Usuário' : 'Novo Usuário' }}</h2>
        <input v-model="currentUsuario.nome" type="text" placeholder="Nome do usuário"
          class="w-[442px] h-10 border border-gray-300 rounded-md px-3 py-2 mb-4" />
        <input v-model="currentUsuario.email" type="email" placeholder="Email"
          class="w-[442px] h-10 border border-gray-300 rounded-md px-3 py-2 mb-4" />

        <!-- Seção Perfil -->
        <div class="space-y-3 mb-5">
          <div class="flex justify-between items-center">
            <span class="text-xs text-font-dash">Perfil</span>
          </div>
          <label v-for="(perfil, idx) in perfis" :key="idx" class="flex justify-between items-center cursor-pointer">
            <span>{{ perfil }}</span>
            <input v-model="currentUsuario.perfil" :value="perfil" type="radio" class="custom-radio" />
          </label>
        </div>

        <!-- Botões de Ação -->
        <div class="flex justify-between gap-2 mt-12">
          <button @click="closeModal"
            class="bg-button-back text-text-button-back h-10 w-[173px] rounded-md cursor-pointer">
            Voltar
          </button>
          <button @click="saveChanges"
            class="bg-button-add text-text-button-add h-10 w-[259px] rounded-md cursor-pointer">
            {{ isEditMode ? 'Salvar Alterações' : 'Adicionar' }}
          </button>
        </div>
      </div>
    </div>
    <div v-if="showModal" class="fixed inset-0 bg-black opacity-50 z-40" @click="closeModal"></div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import imgPlus from '@/assets/square-plus.svg';
import imgEdit from '@/assets/edit.svg';

const showModal = ref(false);
const isEditMode = ref(false);

const currentUsuario = ref({
  nome: '',
  email: '',
  perfil: 'Desenvolvedor',
});

const usuarios = ref([
  {
    nome: 'Junior Luiz',
    email: 'junior@convicti.com.br',
    perfil: 'Admin',
    status: 'Ativo',
  },
  {
    nome: 'Joao Lucas',
    email: 'joao.lucas@convicti.com.br',
    perfil: 'Desenvolvedor',
    status: 'Ativo',
  },
  {
    nome: 'Cíntia Lopes',
    email: 'cintia.lopes@convicti.com.br',
    perfil: 'Recursos Humanos',
    status: 'Ativo',
  },
]);

const perfis = [ 'Admin', 'Desenvolvedor', 'Recursos Humanos' ];

function openModalForAdd() {
  isEditMode.value = false;
  currentUsuario.value = {
    nome: '',
    email: '',
    perfil: 'Desenvolvedor',
  };
  showModal.value = true;
}

function openModalForEdit(usuario) {
  isEditMode.value = true;
  currentUsuario.value = { ...usuario };
  showModal.value = true;
}

function closeModal() {
  showModal.value = false;
}

function saveChanges() {
  if (!currentUsuario.value.nome || !currentUsuario.value.email || !currentUsuario.value.perfil) {
    alert('Por favor, preencha todos os campos.');
    return;
  }

  if (isEditMode.value) {
    const index = usuarios.value.findIndex((u) => u.email === currentUsuario.value.email);
    if (index !== -1) {
      usuarios.value[ index ] = { ...currentUsuario.value, status: 'Ativo' };
    }
  } else {
    usuarios.value.push({ ...currentUsuario.value, status: 'Ativo' });
  }

  closeModal();
}
</script>

<style scoped>
.custom-radio {
  appearance: none;
  width: 20px;
  height: 20px;
  border: 2px solid #000;
  border-radius: 50%;
  outline: none;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
}

.custom-radio:checked {
  border-color: #7F43FF;
}

.custom-radio::after {
  content: '';
  display: block;
  width: 10px;
  height: 10px;
  background-color: transparent;
  border-radius: 50%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  transition: background-color 0.3s ease;
}

.custom-radio:checked::after {
  background-color: #7F43FF;
}
</style>