<template>
  <div id="burger-table" v-if="this.burgers">
    <!-- header da tabela -->
    <div>
      <div id="burger-table-heading">
        <div class="order-id">Nº:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>

    <!-- linhas da tabela para cada pedido  -->
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
        <div class="order-number">{{ burger.id }}</div>
        <div>{{ burger.nome }}</div>
        <div>{{ burger.pao }}</div>
        <div>{{ burger.carne }}</div>
        <div>
          <!-- como os opcionais nao possuem id, é nescessario do index
                para preencher o key -->
          <ul v-for="(opcional, index) in burger.opcionais" :key="index">
            <li>{{ opcional }}</li>
          </ul>
        </div>

        <!-- atualizar status do pedido, poderia posteriormente criar uma condicao
            para diferenciar o acesso desta opção para um gerente ou cliente -->
        <div>
          <select
            name="status"
            class="status"
            @change="updateBurger($event, burger.id)"
          >
            <!-- o selected faz uma comparacao com o status do hamburguer selecionado
                se o status dele é igual ao tipo do statusAtual, se for igual
                irá ser feita uma selecao da opcao, fazendo com q todos os hamburgueres
                fiquem como "solicitados", que é o que o banco ja inicia novos pedidos -->
            <option
              :value="statusAtual.tipo"
              v-for="statusAtual in status"
              :key="statusAtual.id"
              :selected="burger.status == statusAtual.tipo"
            >
              {{ statusAtual.tipo }}
            </option>
          </select>

          <!-- removendo o pedido  -->
          <button class="delete-btn" @click="deleteBurger(burger.id)">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <h1>Não existem pedidos no momento!</h1>
  </div>
</template>
<script>
export default {
  name: "Dashboard",

  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
    };
  },

  methods: {
    // resgatando pedidos ja cadastrados no banco
    async getPedidos() {
      const requisicaoGetPedidos = await fetch("http://localhost:3000/burgers");

      const data = await requisicaoGetPedidos.json();

      this.burgers = data;

      // obtendo status dos pedidos
      this.getStatus();
    },

    // resgatando os 3 tipos de status que existem no banco para
    //     o front apenas iterar sobre eles
    async getStatus() {
      const requisicaoGetStatus = await fetch("http://localhost:3000/status");

      const data = await requisicaoGetStatus.json();

      this.status = data;
    },

    // removendo pedido do banco de dados
    async deleteBurger(id) {
      // informa a requisicao que o metodo a ser aplicado sobre o requisicaoDeleteHamburguer
      //     com esse id seja deletado da base de dados
      const requisicaoDeleteHamburguer = await fetch(
        `http://localhost:3000/burgers/${id}`,
        {
          method: "DELETE",
        }
      );

      const responseDeleteHamburguer = await requisicaoDeleteHamburguer.json();

      // resgata os pedidos novamente para atualizar pagina
      this.getPedidos();
    },

    // atualizando o status do pedido 
    async updateBurger(event, id) {
      const option = event.target.value;

      const dataJson = JSON.stringify({ status: option });

      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      console.log(res);
    },
  },

  mounted() {
    this.getPedidos();
  },
};
</script>

<style scoped>
#burger-table {
  max-width: 1200px;
  /* centralizar tabela  */
  margin: 0 auto;
}

/* definindo para que um item fique do lado do outro  */
#burger-table-heading,
#burger-table-rows,
.burger-table-row {
  display: flex;
  flex-wrap: wrap;
}

#burger-table-heading {
  font-weight: bold;
  padding: 12px;
  /* divisao do heading para o resto da tabela  */
  border-bottom: 3px solid #333;
}

.burger-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}

/* divide igualmente as larguras das colunas */
#burger-table-heading div,
.burger-table-row div {
  width: 19%;
}

/* definindo que a primeira coluna do numero do pedido possua uma largura menor  */
#burger-table-heading .order-id,
.burger-table-row .order-number {
  width: 5%;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
}

.delete-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>