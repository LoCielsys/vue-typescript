<template>
  <div class="hello">
    <h1>{{ name }}</h1>
    {{ msg }}
    <div>
    	<input type="text" v-model="num">
    	{{ num }}
    	<input type="button" @click="setStringToNum">
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
// import Component from 'vue-class-component'
import { Component, Prop } from 'vue-property-decorator'

@Component
export default class Hello extends Vue {
  // list: string[] = []
  @Prop(String) name!: string;
  msg: string = 'Welcome to Your Vue.js App';
  num: number = 1;

  setStringToNum(){
  	// this.num = this.msg;
  }

}

/*export default Vue.extend({
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      num: string = 123
    }
  }
})*/
/*
export default {
  name: 'Hello',
  data () {
    return {
      msg: 'This is a typescript project now.'
    }
  }
}*/
</script>
