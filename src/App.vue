<template>
<main>
  <h3><span v-html="emoji"></span> kanye quotes</h3>
  <a href="https://api.kanye.rest/">https://api.kanye.rest/</a>
  <div class="card">
    <h4>"{{quote}}"</h4>
    <p>- Kanye West</p>
  </div>
  <button @click="gerarQuote">🔄 gerar</button>
  <div class="quoteToggle">
    <button @click="shuffle">➡️ Quote List</button>
    <div v-if="toggle" class="quotesList">
      <div v-for="quotes in list" :key="quotes">
        <ul>
          <li>{{quotes}}</li>
        </ul>
      </div>
    </div>
  </div>
</main>
</template>

<script>
import axios from 'axios'
import quotesList from './quotes.json'
export default {
  data(){
    return{
      quote: '',
      emoji:'',
      list: quotesList,
      toggle: false
    }
  },
  methods:{
    gerarQuote(){
      axios
      .get('https://api.kanye.rest/')
      .then(resp => this.quote = resp.data.quote)

      axios
      .get('https://emojihub.herokuapp.com/api/random')
      .then(emoji => this.emoji = emoji.data.htmlCode[0])
    },
    shuffle(){
      this.toggle = !this.toggle
      this.list.sort(() => Math.random() - 0.5)
    }
  },
  mounted(){
    this.gerarQuote()
  }
}
</script>

<style lang="scss">
@import '@/assets/_shared.scss';
*{
  box-sizing: border-box;
  font-family: 'Helvetica';
}

body{
  margin: 0 auto;
  padding: 1rem;
  max-width: calc(550px + 2rem);
}

main{
  h3{
    text-align: center;
    margin-bottom: 50px;
  }

  a{
   text-decoration: none;
   color: rgb(0, 100, 200)
  }
}



.card, .quoteToggle {
  position: relative;
  padding: 1rem;
  background-color: $bg-color;
  border: $border;
  border-radius: 10px;
  margin: 20px 0;

  p{
    position: absolute;
    right: 15px;
    bottom: 0;
  }
}

.quotesList{
  height: 35vh;
  overflow-y: scroll;
}

ul{
  padding: 0;
  margin: 20px;

  li{
    border-bottom: 1px solid #999;
    list-style-type:none;
    opacity:0.85
  } 
}


button{
  padding: 1rem;
  border: none;
  outline: none;
  cursor: pointer;
  background-color: transparent;
  font-family: inherit;
  font-size: 0.8rem;
  font-weight: 500;
  padding-left: 0;
}

</style>