<script>
import axios from "axios";

export default {
  data() {
    return {
      jogadores: [],
      times: [],
      jogador: {},
    };
  },
  async created() {
    await this.buscarTodosOsJogadores();
    await this.buscarTodosOsTimes();
  },
  methods: {
    async buscarTodosOsTimes() {
      const times = await axios.get("http://localhost:4000/times");
      this.times = times.data;
    },
    async buscarTodosOsJogadores() {
      const jogadores = await axios.get(
        "http://localhost:4000/jogadores?expand=time"
      );
      this.jogadores = jogadores.data;
    },
    async salvar() {
      await axios.post("http://localhost:4000/jogadores", this.jogador);
      await this.buscarTodosOsJogadores();
    },

    excluir(jogador) {
      const indice = this.jogadores.indexOf(jogador);
      this.jogadores.splice(indice, 1);
    },
  },
};
</script>
<template>
  <div class="container">
    <div class="title">
      <h2>Gerenciamento de jogadores</h2>
    </div>
    <div class="form-input">
      <input
        type="text"
        placeholder="Nome"
        v-model="jogador.nome"
        @keyup.enter="salvar"
      />
      <input
        type="text"
        placeholder="Time"
        v-model="jogador.timeId"
        @keyup.enter="salvar"
      />
      <input
        type="text"
        placeholder="Idade"
        v-model="jogador.idade"
        @keyup.enter="salvar"
      />

      <button @click="salvar">salvar</button>
    </div>

    <div class="list-itens">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Nome</th>
            <th>Idade</th>
            <th>Time ID</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="jogador in jogadores" :key="jogador.id">
            <td>{{ jogador.id }}</td>
            <td>{{ jogador.nome }}</td>
            <td>{{ jogador.idade }}</td>
            <td>{{ jogador.timeId }}</td>
            <td>
              <button>Editar</button>
              <button @click="excluir(jogador)">Excluir</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<style scoped></style>
