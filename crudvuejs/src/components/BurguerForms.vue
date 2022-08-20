<template>
  <div>
    <Message :mensagem="this.mensagem" v-show="this.mensagem" />
    <div>
      <form id="burger-form" @submit="createBurguer">
        <!-- informacoes nome -->
        <div class="input-container">
          <label for="nome">Nome do cliente:</label>
          <input
            type="text"
            id="nome"
            name="nome"
            v-model="nome"
            placeholder="Informe o seu nome!"
          />
        </div>

        <!-- informar o pao -->
        <div class="input-container">
          <label for="pao">Escolha o pão:</label>
          <select name="pao" id="pao" v-model="pao">
            <option value="">Selecione o seu pão.</option>
            <option v-for="pao in this.paes" :key="pao.id" :value="pao.tipo">
              {{ pao.tipo }}
            </option>
          </select>
        </div>

        <!-- informar a carne -->
        <div class="input-container">
          <label for="carne">Escolha a carne do seu Burguer:</label>
          <select name="carne" id="carne" v-model="carne">
            <option value="">Selecione o tipo de carne.</option>
            <option
              v-for="carne in this.carnes"
              :key="carne.id"
              :value="carne.tipo"
            >
              {{ carne.tipo }}
            </option>
          </select>
        </div>

        <!-- selecionar opcionais -->
        <div id="opcionais-container" class="input-container">
          <label id="opcionais-title" for="opcionais "
            >Selecione os opcionais:</label
          >
          <div
            class="checkbox-container"
            v-for="opcional in this.opcionaisdata"
            :key="opcional.id"
          >
            <input
              type="checkbox"
              name="opcionais"
              v-model="opcionais"
              :value="opcional.tipo"
            />
            <span>{{ opcional.tipo }}</span>
          </div>
        </div>

        <!-- botao de enviar dados -->
        <div class="input-container">
          <input type="submit" class="submit-btn" value="Criar meu Burger." />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "BurguerForms",
  data() {
    return {
      paes: null,
      carnes: null,
      opcionaisdata: null,
      nome: null,
      pao: null,
      carne: null,
      opcionais: [],
      mensagem: null,
    };
  },
  components: {
    Message,
  },
  methods: {
    async getIngredientes() {
      // banco de dados local criado para criar API com JSON server
      const requisicaoIngredientes = await fetch(
        "http://localhost:3000/ingredientes"
      );
      const data = await requisicaoIngredientes.json();

      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionaisdata = data.opcionais;
    },

    // o metodo create recebe um argumento "e" para que a aplicação pare
    //   o evento do formulario quando clicar no botao "submit"
    async createBurguer(e) {
      e.preventDefault();

      // objeto com todos os dados que foram informados no form
      const data = {
        nome: this.nome,
        carne: this.carne,
        pao: this.pao,
        opcionais: Array.from(this.opcionais),
        // os hamburgues sempre irao chegar primeiro no backend como solicitados
        //   nunca como preparando ou finalizado
        status: "Solicitado",
      };

      // o proximo passo é transformar o objeto data para um texto
      const dataJson = JSON.stringify(data);

      // enviando uma requisicao POST para a api
      const requisicaoEnviarForm = await fetch(
        "http://localhost:3000/burgers",
        {
          method: "POST",
          // informar que a comunicacao é feita por json
          headers: { "Content-Type": "application/json" },
          body: dataJson,
        }
      );

      const respostaRequisicaoEnviarForm = await requisicaoEnviarForm.json();

      // console.log(respostaRequisicaoEnviarForm);

      // mensagem do sistema a ser mostrada acima do form
      this.mensagem = `Pedido Nº: ${respostaRequisicaoEnviarForm.id} realizado com sucesso!`;

      // limpando dados
      this.nome = "";
      this.carne = "";
      this.pao = "";
      this.opcionais = "";

      // limpando mensagem
      setTimeout(() => (this.mensagem = ""), 5000);
    },
  },
  mounted() {
    this.getIngredientes();
  },
};
</script>

<style scoped>
#burger-form {
  max-width: 400px;
  /* centralizar na tela  */
  margin: 0 auto;
}
.input-container {
  display: flex;
  /* com o valor colum a label fica em uma linha e o input na outra  */
  flex-direction: column;
  margin-bottom: 20px;
}
label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  /* esse padding cria um espacamento apra a bordar ficar um pouco
    mais alta e distante do texto */
  padding: 5px 10px;
  /* essa borda é o detalhe na cor laranja ao lado do texto */
  border-left: 4px solid #fcba03;
}
input,
select {
  padding: 5px 10px;
  width: 300px;
}
#opcionais-container {
  /* definindo para a label de cada checkbox fique do lado do mesmo  */
  flex-direction: row;
  flex-wrap: wrap;
}
#opcionais-title {
  width: 100%;
}
.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}
.checkbox-container span,
.checkbox-container input {
  width: auto;
}
.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}
.submit-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  /* quando passar o mouse em cima do botao o icone de seta
    sera alterado para a maozinha; */
  cursor: pointer;
  transition: 0.5s;
}
.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>