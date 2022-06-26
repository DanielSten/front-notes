<template>
  <div class="content flex">
    <div class="left_menu"></div>
    <div class="block_notes">
      <div class="block_notes_input_zone">
        <textarea class="block_notes_input_zone_tittle" v-model="title"></textarea><br>
        <textarea class="block_notes_input_zone_subtittle" v-model="text"></textarea>
        <button @click="addNote" class="block_notes_input_zone_button">Отправить</button>
      </div>
      <div class="block_notes_exists flex">
        <note v-for="note of notes" :note="note" @after-delete="loadNotes" @onDeletePressed="deleteNote" @onSavePressed="editNote"></note>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .flex{
    display: flex;
  }
  .left_menu{
    height: 100vh;
    width: 250px;
    background-color: #5e5e5e;
  }
  .block_notes{
    background-color: #d2d2d2;
    width: 100%;
    height: 100vh;
  }
  .block_notes_input_zone{
    margin: 20px 50px;
  }
  .block_notes_input_zone_tittle{
    font-weight: bold;
    font-size: 24px;
    padding: 10px;
    width: 480px;
    height: 48px;
  }
  .block_notes_input_zone_subtittle{
    font-size: 20px;
    padding: 10px;
    width: 480px;
    height: 80px;
  }
  textarea{
    resize: none;
  }
  .block_notes_input_zone_button{
    height: 40px;
    width: 150px;
    margin: 5px 50px;
  }
  .block_notes_exists{
    width: 100%;
    background-color: #d3bdbd;
    flex-wrap:wrap;
  }
</style>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import Note from '@/components/note'
import Axios from 'axios'

export default {
  name: 'HomeView',
  components: {
    HelloWorld,
    Note,
  },
  mounted() {
    this.loadNotes()
  },
  data(){
    return {
      notes: [],
      title: '',
      text: '',
    }
  },
  methods:{
    addNote(){
      Axios.post('http://backend-project/app-1/note/create', {
        "title": this.title,
        "text": this.text
      })
          .then(res => {
            if(res.data._status){
              this.title = '';
              this.text = '';
              this.loadNotes()
            }
          })
          .catch(e => console.error(e))
    },
    loadNotes(){
      Axios.get('http://backend-project/app-1/notes')
          .then(res => {
            if(res.data._status){
              this.notes = res.data.notes
            }
          })
          .catch(e => console.error(e))
    },
    deleteNote(id){
      Axios.post('http://backend-project/app-1/note/'+ id +'/delete')
          .then(res => {
            if(res.data._status){
              this.loadNotes()
            }
          })
    },
    editNote(note){
      Axios.post('http://backend-project/app-1/note/'+ note.id +'/update', {
        "title": note.title,
        "text": note.text
      })
          .then(res => {
            if(res.data._status){

            }
          })
          .catch(e => console.error(e))
    },
  },

}
</script>
