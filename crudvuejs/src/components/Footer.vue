<template>
    <footer id="footer">
        <!-- para colocar o simbolo de copyright basta adicionar &copy;  -->
        <p>Make your Burguer! &copy; 2021</p>
    </footer>
</template>

<script>
export default {
    name: "Footer"
}
</script>

<style scoped>
footer{
    height: 200px;
    background-color: #222;
    border-top: 4px solid #111;
    color: #FCBA03;
    display: flex;
    align-items: center;
    justify-content: center;
    /* fixando ele no final da pagina  */
    bottom: 0;
    /* vai ocupar toda area nescessaria */
    width: 100%;
}
</style>