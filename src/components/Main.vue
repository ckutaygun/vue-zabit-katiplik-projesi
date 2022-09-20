<template>

  <!-- Header-->
  <header class="masthead text-center text-white">
    <div class="masthead-content">
      <div class="container px-5">
        <h1 class="masthead-heading mb-0">Zabıt kâtipleri hızlı yazma alıştırması </h1>
        <h5 class="masthead-subheading mb-0">Parmaklarına ve kendine güveniyor musun?</h5>
        <a class="btn btn-primary btn-xl rounded-pill mt-5" href="#scroll">Kendini Test Et</a>
      </div>
    </div>
    <div class="bg-circle-1 bg-circle"></div>
    <div class="bg-circle-2 bg-circle"></div>
    <div class="bg-circle-3 bg-circle"></div>
    <div class="bg-circle-4 bg-circle"></div>
  </header>
  <!-- Content section 1-->
  <section id="scroll">
    <div class="container px-5">
      <div class="row gx-5 align-items-center">

        <div class="col-lg-12 order-lg-1">
          <div class="p-5">
            <div class="timer" >
              <button class="timer-button btn btn-primary" disabled type="button">{{ timer }} sn.</button>
              <br><br>
            </div>
            <div class="container">
              <h2>Hızlı Yazma Yarışması</h2>

              <div class="mt-4 p-5 bg-primary text-white rounded">
                <h1>Ne kadar hızlı klavye kullandığını test et</h1>

                <div v-if="isFinish" class="alert alert-success">
                  <h1>Süre Bitti</h1>
                  <p class="display-4"> {{ dks }} DKS</p><br>
                  <span>Doğruluk Yüzdesi : % {{ truePercent}}</span><br>
                  <span>Doğru Kelime : {{ trueCount}}</span><br>
                  <span>Yanlış Kelime : {{ falseCount}}</span><br>
                  <button @click="newGame" class="btn btn-success btn-block">Yeni Oyun</button>
                </div>
                <div v-else class="card">
                  <div class="card-body text-black">
                    <span v-for="(word,key) in words.filter((data, index) => index < 20)" :key = "key" v-bind:class="key!=0 || writingWordControl" class="words">{{ word }}</span>
                    <hr style="color:black">
                  </div>

                  <div class="card-body bg-secondary">
                    <div class="input-group">
                      <input type="text" class="form-control" v-model="writingWord" >
                      <button class="btn btn-primary" disabled type="button">{{ timer }} sn.</button>
                      <button :disabled="isRunning" class="btn btn-info" type="button" @click="getWords()">Yenile</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>

          </div>
        </div>
      </div>
    </div>
  </section>
  <!-- Content section 2-->

  <!-- Footer-->
  <footer class="py-5 bg-black">
    <div class="container px-5"><p class="m-0 text-center text-white small"><a href="https://ckutaygun.github.io/" target="_blank">Copyright &copy; Kutay Aygün 2022</a></p></div>
  </footer>
</template>

<script>
      import wordList from '@/assets/words.json'

export default {
  name: 'App',
  data () {
    return {
      words: ['elma', 'muz', 'armut', 'kutay', 'ahmet'],
      writingWord: null,
      isTrue: true,
      trueCount: 0,
      falseCount: 0,
      timer: 60,
      interval: false,
      isRunning: false,
      isFinish: false,
      wordList: wordList
    }
  },
  watch: {
    writingWord (val) {
      if (!val || val === ' ') {
        this.writingWord = ''
        return
      }
      if (!this.isRunning) this.toggleTimer()
      const word = this.words[0].slice(0, val.length)
      const userWord = val.replace(' ', '')
      this.isTrue = word === userWord
      if (val.indexOf(' ') !== -1) {
        this.isTrue ? this.trueCount++ : this.falseCount++
        this.words.splice(0, 1)
        this.writingWord = ''
        this.isTrue = true
      }
    }
  },
  computed: {
    writingWordControl () {
      return this.isTrue ? 'writing-word' : 'writing-word bg-danger'
    },
    dks () {
      return 300 - this.words.length
    },
    truePercent () {
      const percent = (100 / this.dks)
      const val = (percent * this.trueCount)
      return isNaN(val) ? 0 : val
    }
  },
  mounted () {
    this.getWords()
  },
  methods: {
    newGame () {
      this.getWords()
      this.isFinish = false
      this.timer = 60
      this.isTrue = true
      this.isRunning = false
      this.writingWord = ''
    },
    getWords () {
      this.words = this.wordList.sort(() => Math.random() - 0.5).splice(0, 300)
    },
    toggleTimer () {
      this.isRunning = true
      this.interval = setInterval(this.timeProcess, 1000)
    },
    timeProcess () {
      if (this.timer === 0) {
        this.isFinish = true
        clearInterval(this.interval)
        return
      }
      this.timer--
    }
  }
}
</script>

<style scoped>
.masthead-subheading{
  font-size: 30px !important;
}

.words{
  font-size: 25px;
  margin-left: 5px;
  font-weight: 400;
}
.writing-word{
  background-color: gray;
  color: white;
  padding: 5px;
  border-radius: 5px;
}
.timer .timer-button{
  font-size: 25px;
  float: right;
  border-radius: 89px;
  width: 100px;
  height: 100px;
}

</style>
