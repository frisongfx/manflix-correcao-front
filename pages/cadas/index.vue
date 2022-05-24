<template>
  <div class="wallpaper">
    <div class="fundo">
      <img class="img1" src="../../static/manflix.png">
      <div class="container1">

        <span class="txt1">Cadastro</span>

        <InputText class="input1" type="text" placeholder="Usuário" v-model="user.username"/>

        <InputText class="input1" type="text" placeholder="Email" v-model="user.email"/>

        <InputText class="input1" type="password" placeholder="Senha" v-model="user.password"/>

        <InputText class="input1" type="password" placeholder="Repetir senha" v-model="user.repeatpassword"/>

        <Button label="Sing In" class="bt1" v-on:click="sendLogin()"/>   

        <span class="txt2" @click="()=>{
        $router.push('/')
        }">Realizar Login</span>

      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: "IndexPage",
  data(){
    return{
      user:{
        username: '',
        password: ''
      }
    }
  },
  methods:{
    sendLogin: function(){
      if(this.user.username && this.user.password){
        
        this.$auth.loginWith(
          "local", {data: this.user}
        ).then(()=>{
          alert("Usuário logado!");
        })

      }
      else{
        
        alert("Preencha todos os campos!");
        
      }
    }
  }
};
</script>

<style lang="css" scoped>

.wallpaper{
  height: 100vh;
  width: 100vw;
  background-image: url('../../static/wallpaper.jpg');
}

.fundo{
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 100vw;
  background-color: rgba(0,0,0, 0.5);
}

.container1{
  width: 30vw;
  padding: 30px;
  background-color: rgba(0,0,0, 0.8);
  display: flex;
  flex-direction: column;
}

.img1{
  position: absolute;
  transform: translate(-780px, -400px) scale(0.16);
}

.txt1{
  color: white;
  font-size: 32px;
  font-family: 'Gill Sans MT';
  margin-bottom: 22px;
}

.input1{
  color: white;
  border: none;
  background-color: #696969;
  margin-bottom: 15px;
  padding: 17px;
}
.input1::placeholder{ color: rgba(255,255,255, 0.6); }

.bt1{
  font-size: 22px;
  font-family: Arial;
  background-color: rgb(196, 0, 0);
  border: none;
}

.bt1:hover{
  background-color: rgb(130, 0, 0);
}

.txt2{
  margin-left: 20vw;
  margin-top: 20px;
  text-decoration: none;
  color: white;
  font-size: 20px;
}

</style>
