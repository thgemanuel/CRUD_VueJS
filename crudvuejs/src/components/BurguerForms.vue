<template>
  <div>
    <p>componen</p>
    <div>
      <form id="burger-form">
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
            <option value="integral">Integral</option>
          </select>
        </div>

        <!-- informar a carne -->
        <div class="input-container">
          <label for="carne">Escolha a carne do seu Burguer:</label>
          <select name="carne" id="carne" v-model="carne">
            <option value="">Selecione o tipo de carne.</option>
            <option value="maminha">maminha</option>
          </select>
        </div>

        <!-- selecionar opcionais -->
        <div id="opcionais-container" class="input-container">
          <label id="opcionais-title" for="opcionais ">Selecione os opcionais:</label>
          <div class="checkbox-container">
            <input
              type="checkbox"
              name="opcionais"
              v-model="opcionais"
              value="salame"
            />
            <span>Salame</span>
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
        status: "Solicitado",
        msg: null,
    }
  },
  methods: {
    
  }
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