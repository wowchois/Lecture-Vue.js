<template>
  <modal>
    <div slot="header">
      <h2>
        Create new board
        <a href="" class="modal-default-button" 
          @click.prevent="SET_IS_ADD_BOARD(false)">&times;</a> <!-- X 표시 -->
      </h2>
    </div>
    <div slot="body">
      <form id="add-board-form" 
        @submit.prevent="addBoard">
        <input class="form-control" type="text" v-model="input" ref="input">
      </form>
    </div>
    <div slot="footer">
      <button class="btn" :class="{'btn-success': valid}" type="submit" form="add-board-form" :disabled="!valid">
        Create Board</button>
    </div>
  </modal>
</template>

<script>
import Modal from './Modal.vue'
import {mapMutations, mapActions} from 'vuex'

export default {
  components: { Modal },
  data () {
    return {
      input: '',
      valid: false
    }
  },
  watch: {
    input(val) {
      this.valid = val.trim().length > 0
    }
  },
  mounted () {
    this.$refs.input.focus()
  },
  methods: {
    ...mapMutations([
      'SET_IS_ADD_BOARD'
    ])
    ,...mapActions([
      'ADD_BOARD'
      ,'FETCH_BOARDS'
    ])
    ,addBoard(){
      this.SET_IS_ADD_BOARD(false)
      this.ADD_BOARD({title : this.input}).then(({id}) => {
        this.$router.push(`/b/${id}`)  //보드 상세화면 이동
        //this.FETCH_BOARDS()
      })
    }
  }
}
</script>

