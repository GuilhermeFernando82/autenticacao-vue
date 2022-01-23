<template>
  <div class="content">
    <div class="container clearfix">
    <h1>Login</h1>
      <input type="text" class="form-control" v-model="Nome" placeholder="Nome"/><br>
      <input type="text" class="form-control" v-model="Password" placeholder="Senha"/><br>
      <button class="btn-success" @click="login">Login</button>
     </div>
  </div>
</template>

<script>
export default({
  name: 'Login',
  props: {
  },
  data: () => ({
    Nome: '',
    Password: '',
    data: [],
  }),
  methods: {
    login(){
      var myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/json");

      var raw = JSON.stringify({
        "name": this.Nome,
        "password": this.Password,
        "role": "Admin"
      });

      var requestOptions = {
        method: 'POST',
        headers: myHeaders,
        body: raw,
        redirect: 'follow'
      };

      fetch("https://localhost:7206/api/Usuario/Login", requestOptions)
       .then(response => response.json())
       .then(response => (this.data = response))
       .then(response => {
         if(response !== undefined){
           console.log('data', this.data);
           sessionStorage.setItem('@web/name', this.data[0].name);
           sessionStorage.setItem('@web/password', this.data[0].password);
           sessionStorage.setItem('@web/token', this.data[1]);
          window.location.href="http://localhost:8080/register";
       } else {
         console.log('Usuário ou senha invalidos!!');
       }
       })
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
