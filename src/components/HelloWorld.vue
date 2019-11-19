<template>
  <div class="hello">
    <h1>Список</h1>
    {{countNotDone}} more to do,
    {{countIsDone}} done
    <br>
    <input v-model="fil">
    <button 
      v-for="btn of btns" :key="btn"
      :class="{'active': activeBtn == btn}"
      @click="changeFilterType(btn)"
    >
      {{btn}} 
    </button>

    <p v-for="el of arrNotDone" :key="el.title" >
      {{el.title}}  
      <button @click="del(el.title)">DEL</button>
      <button @click="done(el)">Done!</button>
    </p>
    <input v-model="inputed">
    <button @click="addHandler">Add</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld', 
  data() {
    return { 
      btns:['All', "Active", 'Done'],
      activeBtn:'All',
      fil:'',
      inputed:'',
      arr: [
        {title: 'Drink coffe', isDone: false},
        {title: 'Make Awesome', isDone: false},
        {title: 'Lunch', isDone: false}
      ]
    }
  },
  computed: {
    arrNotDone(){
      return this.arr.filter(el => {
        let f = true
        if(this.fil) {
           f = el.title.toLowerCase().match(this.fil.toLowerCase())
        }
        let act = true;
        // if(this.activeBtn === 'All') 
        if(this.activeBtn === 'Active') act = !el.isDone  
        if(this.activeBtn === 'Done') act = el.isDone
        return  act && f
      })
    },
    countNotDone() {
      return this.arr.filter(el=>!el.isDone).length
    },
    countIsDone(){
      return this.arr.filter(el=>el.isDone).length
    }
  },
  methods: {
    del(title) {
      const i = this.arr.findIndex(el=>el.title == title)
      this.arr.splice(i,1) 
    },
    done(el){ 
      el.isDone = true
    },
    addHandler(){
      this.arr.push({
        title: this.inputed,
        isDone: false
      })
      this.inputed = ''
    },
    changeFilterType(e) {
      this.activeBtn = e;
    }
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
.active {
  background: green;
  color: white
  
}
</style>
