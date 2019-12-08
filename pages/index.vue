<template>
  <div class="container">
    <span style="color:white">% {{ totalPercent }} başarılı oldunuz.</span>
    <div class="box-container">
      <div v-for="(q, index) in questions" :key="index" class="box">
        <span v-if="!isAnswer" v-text="q.q"></span>
        <input
          :id="'input' + index"
          :name="'input' + index"
          :ref="'input' + index"
          v-model="q.a"
          v-else
          @keydown="valueChanging"
          @keyup="valueChanged"
          @paste="valueChanging"
          type="text"
          class="input"
        />
        <span
          class="answer"
          :style="{ color: q.m === true ? 'green' : 'red' }"
          v-if="q.m !== null"
          v-text="q.q"
        ></span>
      </div>
    </div>

    <div style="width:30%">
      <button @click="answer" class="btn">Cevapla</button>
    </div>
  </div>
</template>

<script>
const REGEXP_NUMBER = /^-?(?:\d+|\d+\.\d+|\.\d+)(?:[eE][-+]?\d+)?$/
export default {
  data() {
    return {
      questions: [],
      isAnswer: false
    }
  },
  mounted() {
    this.prepareQuestions()
  },
  methods: {
    prepareQuestions() {
      const allNumbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

      allNumbers
        .sort(() => Math.random() - 0.5)
        .slice(0, 5)
        .map((v) => this.questions.push({ q: v.toString(), a: null, m: null }))

      setTimeout(() => {
        this.isAnswer = true
        this.$nextTick(() => {
          this.$refs.input0[0].focus()
        })
      }, 2000)
    },
    valueChanging(e) {
      if (![8, 46, 37, 38, 39, 40].includes(e.keyCode)) {
        if (e.target.value.length > 0 || !REGEXP_NUMBER.test(e.key)) e.preventDefault()
      }
    },
    valueChanged(e) {
      if (e.target.parentNode.nextElementSibling)
        e.target.parentNode.nextElementSibling.childNodes[0].focus()
    },
    answer() {
      this.questions.map((v) => (v.m = v.q === v.a))
      console.log(this.questions)
    }
  },
  computed: {
    totalPercent() {
      return (this.questions.filter((t) => t.m === true).length * 100) / 5
    }
  }
}
</script>

<style>
.container {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  flex-direction: column;
  background-color: rgb(73, 73, 73);
}
.box-container {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  flex-direction: row;
}
.answer {
  position: absolute;
  right: 0;
  bottom: 0;
  font-size: 0.3em !important;
}
.box {
  width: 150px;
  height: 150px;
  box-shadow: 0 0 10px rgb(27, 27, 27);
  margin: 10px;
  position: relative;
  font-size: 6em;
  background-color: rgb(230, 230, 230);
  border-radius: 5px;
}
.input {
  background-color: inherit;
  outline: none;
  border: none;
  width: 100%;
  height: 100%;
  text-align: center;
  font-size: inherit;
  font-family: inherit;
}
.btn {
  width: 100%;
  height: 70px;
  font-size: 2em;
  background-color: #007bff;
  color: white;
  border-radius: 10px;
  outline: none;
  border: 1px solid #0070e7;
}
.btn:hover {
  background-color: rgb(77, 154, 255);
  cursor: pointer;
}
</style>
