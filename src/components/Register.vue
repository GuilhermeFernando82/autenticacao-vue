<template>
  <div class="content">
    <div class="container clearfix">
    <h1>Bem vindo {{ username }}</h1>
    <h1>Cadastro de Usuários</h1>
      <input type="text" class="form-control" v-model="Name" placeholder="Nome"/><br>
      <input type="text" class="form-control" v-model="Password" placeholder="Senha"/><br>
      <input type="text" class="form-control" v-model="Role" placeholder="Role"/><br>
      <button class="btn-success" @click="register">Cadastrar</button>
     </div>
  </div>
</template>

<script>
export default({
  name: 'Register',
  props: {
  },
  data: () => ({
    username: sessionStorage.getItem('@web/name'),
    Name: '',
    Password: '',
    Role: '',
  }),
  created(){
    if (sessionStorage.getItem('@web/token') === null) {
      window.location.href="http://localhost:8080/login";
    }
  },
  methods: {
    register(){
      var myHeaders = new Headers();
      myHeaders.append("Authorization", "Bearer "+sessionStorage.getItem('@web/token')+"");
      myHeaders.append("Content-Type", "application/json");
      var raw = JSON.stringify({
        "name": this.Name,
        "password": this.Password,
        "role": "Admin"
      });

      var requestOptions = {
        method: 'POST',
        headers: myHeaders,
        body: raw,
        redirect: 'follow'
      };

      fetch("https://localhost:7206/api/Usuario/authenticated", requestOptions)
        .then(response => response.text())
        .then(result => console.log(result))
        .catch(error => console.log('error', error));
          }
        }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  justify-content: center;
  width: 500px;
  height: 500px;
}
h3 {
  margin: 40px 0 0;
}
input {
  margin: 5px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
