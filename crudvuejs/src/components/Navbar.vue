<template>
  <nav>
    <router-link to="/" id="logo-url">
      <img :src="logo" :alt="alt" id="logo" />
    </router-link>
    <router-link to="/">Home</router-link>
    <router-link to="/pedidos">Pedidos</router-link>
  </nav>
</template>

<script>
export default {
  name: "Navbar",
  props: ["logo", "alt"],
};
</script>

<style scoped>
nav {
  /* cor de fundo */
  background-color: #222;
  border-bottom: 4px solid #111;
  /* padding de 15 no top e bottom e 50 nas laterais */
  padding: 15px 50px;
  /* flex box */
  display: flex;
  /* justificar tudo para a direita */
  justify-content: flex-end;
  /* itens alinhados no centro na vertical */
  align-items: center;
}

/* alinhando a logo para a esquerda */
nav #logo-url {
  margin: auto;
  margin-left: 0;
}

/* definindo tamanho maximo para a logo */
#logo{
    width: 40px;
    height: 40px;
}

/* editando texto dentro da barra de navegacao */
nav a{
    color: #FCBA03;
    /* removendo undeline dos itens  */
    text-decoration: none;
    margin: 12px;
    transition: 0.5s;
}

/* o que acontece quando passa o mouse por cima dos links */
nav a:hover{
    color: #FFF;
}
</style>