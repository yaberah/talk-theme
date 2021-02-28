<template>
  <div id="app">
    <h1 class="title">
      <img class="title__inner" src="./assets/title.svg" alt="">
    </h1>
    <div class="setting"  v-show=" mode === 'setting'">
      <ul class="setting__list">
        <li class="setting__item" v-for="member in members" :key="member.id">{{ member.name }}<button class="setting__delete" @click="deleteMember(member)">×</button></li>
      </ul>
      <input class="setting__input" type="text" v-model="name" placeholder="名前をいれよう">
      <button class="setting__add-button" @click="addMember">メンバーを追加する</button>
      <button class="setting__start-button" @click="runApp">START</button>
    </div>
    <div class="result" v-show=" mode === 'result'">
      <p class="result__text">話す人は...</p>
      <p class="result__member">{{ result.member }}</p>
      <p class="result__text">テーマは...</p>
      <p v-for="theme in result.themes" :key="theme" class="result__theme">{{ theme }}</p>
      <button class="result__back-button" @click="backTop">もどる</button>
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
        themes: [],
      }
    }
  },
  computed: {
    list: function(){
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
      const themeIndex1 = Math.floor( Math.random() * this.list.length );
      const themeIndex2 = Math.floor( Math.random() * this.list.length );
      const themeIndex3 = Math.floor( Math.random() * this.list.length );

      this.result.member = this.members[memberIndex].name;
      this.result.themes.push(this.list[themeIndex1]);
      this.result.themes.push(this.list[themeIndex2]);
      this.result.themes.push(this.list[themeIndex3]);
    },
    backTop: function() {
      this.result.member = '';
      this.result.themes = [];
      this.mode = 'setting';
    }
  },
}
</script>


<style lang="scss">


html{
  background:#0D555F;
}
</style>

<style lang="scss" scoped>
$background-color: #0D555F;
$list-background-color:rgba(255,255,255,0.1);
$letter-color:#FFFFFF;
$primary-color:#C49F24;
$secondary-color:#37A87D;
$placeholder: #DEDEDE;
$shadow: rgba(0,0,0,0.1);

%common-format{
  width: 310px;
  height: 64px;
  border-radius: 10px;
  font-size: 22px;
  font-weight: bold;
  line-height: 64px;
  padding: 0 16px;
  box-sizing: border-box;
  border: none; 
  margin: 4px auto;
}
#app{
  width: 375px;
  margin: 48px auto 0;
}
.title{
  text-align: center;
}
.setting{
  display: flex;
  flex-direction: column;
  &__list{
    padding: 0;
  }
  &__item{
    @extend %common-format;
    width: 310px;
    height: 64px;
    background: $list-background-color;
    border-radius: 10px;
    list-style: none;
    color: $letter-color;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  &__delete{
    background:$shadow ;
    border: none;
    border-radius: 100px;
    width: 28px;
    height: 28px;
    color: $letter-color;
  }
  &__input{
    @extend %common-format;
    &::placeholder {
      color: $placeholder;
    }
  }
  &__add-button{
    @extend %common-format;
    background-color: $secondary-color;
    color: $letter-color;
  }
  &__start-button{
    @extend %common-format;
    background-color: $primary-color;
    color: $letter-color;
  }
}
.result{
  display: flex;
  flex-direction: column;
  
  &__text{
    padding: 0 32px;
    font-weight: bold;
    font-size: 18px;
    color:$letter-color;
  }
  &__member{
    @extend %common-format;
    background-color: $letter-color;
  }
  &__theme{
    background-color: $letter-color;
    width: 310px;
    border-radius: 10px;
    font-size: 18px;
    font-weight: bold;
    height: 100px;
    padding: 0 16px;
    box-sizing: border-box;
    border: none; 
    margin: 4px auto;
    display: flex;
    align-items: center;
  }
  &__back-button{
    @extend %common-format;
    background-color: $primary-color;
    color: $letter-color;
  }
}
</style>


