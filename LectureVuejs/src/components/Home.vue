<template>
  <div>
    <div class="home-title">Personal Boards</div>
    <div class="board-list" ref="boardList">
      <div class="board-item" v-for="b in boards" :key="b.id" :data-bgcolor="b.bgColor" ref="boardItem">
        <router-link :to="`/b/${b.id}`">
          <div class="board-item-title">{{b.title}}</div>
        </router-link>
      </div>
      <div class="board-item board-item-new">
          <a href="" class="new-board-btn" @click.prevent="SET_IS_ADD_BOARD(true)">
          Create new board...
        </a>
      </div>
    </div>
    <AddBoard v-if="isAddBoard" />
  </div>
</template>

<script>
import AddBoard from './AddBoard.vue'
import {mapState, mapMutations, mapActions} from 'vuex'

export default {
  components : {AddBoard}, //컴포넌트 등록
  data(){
    return {
      loading : false
      ,error : ''
    }
  }
  ,created(){
    this.fetchData()
    this.SET_THEME() //body, navbar 초기 색상 세팅
  }
  ,computed : {
    ...mapState([
      'isAddBoard'
      ,'boards'
    ])
  }
  ,updated() { 
    this.$refs.boardItem.forEach(el => {
      el.style.backgroundColor = el.dataset.bgcolor
    })
  }
  ,methods : {
    ...mapMutations([
      'SET_IS_ADD_BOARD'
      ,'SET_THEME'
    ])
    ,...mapActions([
      'FETCH_BOARDS'
    ])
    ,fetchData(){
      this.loading = true
      this.FETCH_BOARDS().finally(() => {
        this.loading = false
      }) //FETCH_BOARDS는 promise 반환해서 .finally 사용가능
    }
  }
}
</script>

<style>
.home-title {
  padding: 10px;
  font-size: 18px;
  font-weight: bold;
}
.board-list {
  padding: 10px;
  display: flex;
  flex-wrap: wrap;
}
.board-item {
  width: 23%;
  height: 100px;
  margin: 0 2% 20px 0;
  border-radius: 3px;
}
.board-item a {
  text-decoration: none;
  display: block;
  width: 100%;
  height: 100%;
}
.board-item a:hover,
.board-item a:focus {
  background-color: rgba(0,0,0, .1);
  color: #666;
}
.board-item-title {
  color: #fff;
  font-size: 18px;
  font-weight: 700;
  padding: 10px;
}
.board-item a.new-board-btn {
  display: table-cell;
  vertical-align: middle;
  text-align: center;
  height: 100px;
  width: inherit;
  color: #888;
  font-weight: 700;
}
</style>
