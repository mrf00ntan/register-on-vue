<template>
  <div id="app">
    <form v-on:submit.prevent="submitForm">
      <div class="alert alert-danger" v-if="error">
        <strong>Ошибка</strong> Неверный E-mail или пароль
      </div>
      <div class="form-group">
        <input class="form-control" id="email" v-model="email" type="email" placeholder='E-mail' required=''>
      </div>
      <div class="form-group">
        <input class="form-control" id="pwd" v-model="password" type="password" placeholder='Пароль' required=''>
      </div>
      <button class="btn btn-default btn-block" type="submit">
        <div class="download" v-if="download"><img src="http://cdn.shark-helmets.com/skin/frontend/shark/default/storelocator/images/loading/loading_big.gif" alt="download"></div>
        <span v-else>Отправить</span>
      </button>
    </form>
  </div>

</template>

<script>
export default {
  name: 'login-vue',
  data () {
    return {
      error: false,
      email: '',
      password: '',
      download: false,
      users: {
        'tester': {
          'email': 'mrf00ntan@gmail.com',
          'name': 'Тестер',
          'surname': 'Тестеров',
          'password': 'passpass'
        },
        'tester2': {
          'email': 'example@gmail.com',
          'name': 'Тестер2',
          'surname': 'Тестеров2',
          'password': '123123'
        }
      }
    }
  },
  methods: {
    submitForm(){
      var email = this.email;
      var password = this.password;
      for(var userKey in this.users){
        var arr = this.users[userKey];
        if(email == arr['email'] && password == arr['password']){
          this.download = true;
          this.$session.set('username', userKey);
          this.$session.set('name', arr['name']);
          this.$session.set('surname', arr['surname']);
          this.$session.set('email', email);
          window.location.replace("/profile");
          break;
        } else {
          this.error = true;
        }
      }
    },
    clearError(){
      this.error = false;
    }
  },
  watch: {
    email: function(){
      this.clearError();
    },
    password: function(){
      this.clearError();
    }
  }
}
</script>

<style lang="scss">
#app{
  button{
    &:focus{
      outline: none;
    }
  }
}

.download{
  width: 20px;
  height: 20px;
  margin: 0 auto;
  outline: none;
  img{
    width: 100%;
    height: 100%;
    outline: none;
    border: none;
  }
}
</style>
