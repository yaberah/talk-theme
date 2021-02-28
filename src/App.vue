<template>
  <div id="app">
    <div class="setting" v-show=" mode === 'setting'">
      <ul class="setting__list">
        <li class="setting__item" v-for="member in members" :key="member.id">{{ member.name }}<button @click="deleteMember(member)">×</button></li>
      </ul>
      <input class="setting__input" type="text" v-model="name" placeholder="名前をいれよう">
      <button class="setting__add-button" @click="addMember">メンバーを追加する</button>
      <button @click="runApp">START</button>
    </div>
    <div class="result" v-show=" mode === 'result'">
      <p class="result__text">話す人は...</p>
      <p class="result__view">{{ result.member }}</p>
      <p class="result__text">テーマは...</p>
      <p class="result__view">{{ result.theme }}</p>
      <button @click="backTop">もどる</button>
    </div>
  </div>
</template>

<script>
import text from "raw-loader!./wordlist.txt";

export default {
  name: 'App',
  data: function() {
    return {
      mode: 'setting',
      name: '',
      members:[],
      result: {
        member: '',
        theme: '',
      }
      }
  },
  computed: {
    themes: function(){
      return text.split('\n')
    }
  },
  methods: {
    addMember: function(){
      if ( this.name.length === 0 ) {
        alert('名前を入力してください');
      } else {
        this.members.push({ name: this.name });
        this.name = '';
      }
    },
    deleteMember: function(member) {
      const index = this.members.indexOf(member);
      this.members.splice(index,1);
    },
    runApp: function() {
      this.mode = 'result';
      const memberIndex = Math.floor( Math.random() * this.members.length );
      const themeIndex = Math.floor( Math.random() * this.themes.length );
      this.result.member = this.members[memberIndex].name;
      this.result.theme = this.themes[themeIndex];
    },
    backTop: function() {
      this.result.member = '';
      this.result.theme = '';
      this.mode = 'setting';
    }
  },
}
</script>

<style lang="scss" scoped>

.setting{
  width: 100%;

}


</style>
