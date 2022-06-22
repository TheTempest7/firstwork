<template>
    <div class="wrapper">
            <div id="popup" ref="popup" class="popup  ">
                <div class="popup__body ">
                    <div  id="popup-content" class="popup__content">
                         <div class="popup__message">
                          id сайта должен содержать 24 символа
                        </div>
                        <div v-on:click="closerPopup" class="popup__closer closePopup">
                            <span class="popup__span"></span>
                            <span class="popup__span1"></span>
                            <span class="popup__span2"></span>
                        </div>
                    </div>
                </div>
            </div>
        <div class="form">
            <form action="#" id="form"  v-on:submit="validation" class="form__body">
                <h1 class="form__title">LeadHit</h1>
                <div class="form__item">
                    <label for="formName" class="form__label">ID*</label>
                    <input id="formName" type="text" name="name"  v-on:input="checkInput" ref="inputId" placeholder="введите id" class="form__input __req">
                </div>
                <button type="submit" class="form__button">Войти</button>
            </form>

        </div>
    </div>
</template>

<script>
import router from '@/router';



export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  methods:{
    closerPopup(){
       const popup=this.$refs.popup;
         popup.classList.remove('open');
    },
      validation(e){
      e.preventDefault();
      console.log(e);
      const df=this.$refs.inputId;
      console.log(typeof df.value);
      async function lol(){
        let c=df.value;
        console.log(c);
          const response= await fetch('https://track-api.leadhit.io/client/test_auth',
        {
          method: 'GET',
          headers: {'Content-Type': 'application/json;charset=utf-8',
          'Api-Key': `5f8475902b0be670555f1bb3:eEZn8u05G3bzRpdL7RiHCvrYAYo`,
          'Leadhit-Site-Id':`${c}`,
            },/**`${df.value}` */
        })
        const result=await response.json()
        console.log(result);
        if(result['message']=='ok'){
          console.log(1213);
          localStorage.setItem('Leadhit-Site-Id',`${c}`);
          router.push('/about');
        }
        }
      if(df.value.length!=24){
        df.classList.add('__error');
        const popup=this.$refs.popup;
        popup.classList.add('open');
      }
      else{
      lol();

        }
      },
    checkInput(e){
       const popup=this.$refs.popup;
       console.log(popup.classList);
      if(popup.classList[1]==='open'){
        popup.classList.remove('open');
      }
      console.log(e.target.value);
    },

  },
  mounted(){

  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
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
.wrapper {
  background:linear-gradient(135deg,black,purple,yellow);
}

.form {
  color: white;
  margin: 0px auto;
  max-width: 600px;
  padding: 40px 0px;
}

.form * {
  outline: none;
}

.form__body {
  position: relative;
}

.form__body::after {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: rgba(51, 51, 51, 0.9) url(//img/png-clipart-jsplaylist-loading-icon-thumbnail.png) center/50px no-repeat;
  opacity: 0;
  visibility: hidden;
}

.form__body.__sending::after {
  opacity: 0.9;
  visibility: visible;
}

.form__title {
  font-size: 40px;
  font-weight: 700;
  margin-bottom: 30px;
}

.form__item {
  margin-bottom: 20px;
}

.form__label {
  font-size: 18px;
  margin-bottom: 10px;
  display: block;
}

.form__input {
  height: 50px;
  padding: 0px 20px;
  width: 100%;
  border-radius: 5px;
  font-size: 18px;
  transition: box-shadow 0.3s ease 0s;
}

.form__input:focus {
  box-shadow: 0 0 15px #42f342;
}

.form__input.__error {
  box-shadow: 0 0 15px #f51505;
}

textarea.form__input {
  min-height: 120px;
  max-width: 100%;
  resize: vertical;
  padding: 20px;
}

.form__button {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: green;
  border-radius: 20px;
  min-height: 60px;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: white;
  font-size: 18px;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 5px 0 #031b03;
  transition: background-color 0.4s ease 0s;
  position: relative;
  top: 0px;
}

.form__button:hover {
  background-color: #143814;
}

.form__button:active {
  top: 3px;
  box-shadow: 0 2px 0 #031b03;
}


.popup {
  background-color: rgba(0, 0, 0, 0.5);
  margin: 0px auto;
  position: fixed;
  width: 282px;
  height: 114px;
  top: 0;
  left: 42%;
  z-index: 100;
  opacity: 0;
  visibility: hidden;
  transition: all 0.8s ease 0.3s;
  -webkit-transition: all 0.8s ease 0.3s;
  -moz-transition: all 0.8s ease 0.3s;
  -ms-transition: all 0.8s ease 0.3s;
  -o-transition: all 0.8s ease 0.3s;
}

.popup.open {
  opacity: 1;
  visibility: visible;
}

.popup__body {
  min-height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.popup__content {
  padding: 57px 141px;
  display: flex;
  flex-direction: column;
  background:linear-gradient(45deg,green,darkblue);
  position: relative;
}
.popup__message{
position: absolute;
left: 0;
display: flex;
color: white;
}
.popup__closer {
  position: absolute;
  display: flex;
  flex-direction: column;
  right: 10px;
  top: 10px;
}

.popup__span1 {
  display: block;
  width: 30px;
  height: 3px;
  background-color: rgb(252, 252, 252);
  margin-top: 10px;
  transform: rotate(45deg) translateY(9px);
  -webkit-transform: rotate(45deg) translateY(9px);
  -moz-transform: rotate(45deg) translateY(9px);
  -ms-transform: rotate(45deg) translateY(9px);
  -o-transform: rotate(45deg) translateY(9px);
}

.popup__span {
  display: none;
  width: 30px;
  height: 1px;
   background-color: rgb(252, 252, 252);
}

.popup__span2 {
  display: block;
  width: 30px;
  height: 3px;
   background-color: rgb(252, 252, 252);
  margin-top: 10px;
  transform: rotate(135deg) translateY(9px);
  -webkit-transform: rotate(135deg) translateY(9px);
  -moz-transform: rotate(135deg) translateY(9px);
  -ms-transform: rotate(135deg) translateY(9px);
  -o-transform: rotate(135deg) translateY(9px);
}

@media (max-width: 1000px) {
  .popup__content {
    padding: 24px 60px;
  }
}
</style>
