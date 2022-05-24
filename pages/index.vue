<template>
  <div class="wallpaper">
    <div class="fundo">
      <img class="img1" src="manflix.png">
      <div class="container1">

      <span class="txt1">Login</span>

      <InputText class="input1" type="text" placeholder="Usuário" v-model="user.username"/>

      <InputText class="input1" type="password" placeholder="Senha" v-model="user.password"/>

      <span class="error">{{ message }}</span>

      <Toast/>

      <Button label="Sing In" class="bt1" v-on:click="sendLogin()"/>   

      <span class="txt2" @click="()=>{
        $router.push('/cadas')
        }">Cadastre-se</span>

      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: "login",
  data(){
    return{
      user:{
        username: '',
        password: ''
      },
      message: ""
    }
  },
  methods:{
    sendLogin: async function(){
      if(this.user.username && this.user.password){
        
        this.$auth.loginWith(
          "local", {data: this.user}
        ).then((response)=>{
          alert("Usuário logado!");
        })
        .catch((response)=>{
          console.log(response);
          this.user.username = "";
          this.user.password = "";

          this.message = "Usuário ou Senha inválidos!";
          setTimeout(()=>{
            this.message = "";
          }, 5000)

          this.$toast.add(
            {
              severity: "error",
              summary: "Erro no Login",
              detail: "Usuário ou Senha inválidos!",
              life: 5000
            }
          );
        })

      }else{ 
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
  background-image: url('../static/wallpaper.jpg');
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
  padding: 50px;
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
  margin-top: 20px;
  text-decoration: none;
  color: white;
  font-size: 20px;
  cursor: pointer;
}

.error{
  color: red;
  margin-bottom: 5px;
}

</style>
