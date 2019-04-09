<template lang="pug">
  div#app
    b-container.cont(fluid)
      h3.title PT POPDIC
      b-input-group.mb-3(@keyup.enter="getDict")
        b-form-input(placeholder="단어를 입력해주세요." v-model="query")
        b-input-group-append
          b-btn(variant="outline-success" :disabled="loading" @click="getDict") 검색

      b-card(v-if="dict.error" title="단어를 찾지 못했습니다.")

      b-card(v-if="dict.entry && !dict.error" :title="dict.entry + ' ' + (dict.phoneticSigns || '')")
        p.card-text(v-for="(m, idx) in dict.meanings" :key="m.value") {{ idx + 1 }}. {{ m.partOfSpeech || '' }} {{ m.value }}
</template>

<script>
import axios from 'axios'

export default {
	name: 'app',
	data () {
		return {
      query: '',
      loading: false,
			dict: {}
		}
  },
  methods: {
    getDict () {
      if (!this.query || this.loading) return

      this.loading = true

      axios.get(`http://ptdic.lento.in/api?query=${this.query}`)
        .then(({ data }) => {
          console.log(data)
          this.dict = data

          this.loading = false
        })
        .catch(e => {
          console.log(e)
          this.dict = { error: true }
          this.loading = false
        })
    }
  }
}
</script>

<style>
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
  width: 400px;
	color: #2c3e50;
}

.cont {
	border: 4px solid #2cc34c;
	border-radius: 8px;
	padding: 10px;  
}

.title {
  text-align: center;  
	color: #2cc34c;
  font-weight: bold;
}
</style>
