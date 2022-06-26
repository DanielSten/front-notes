<template>
  <div class="note">
    <div v-if="!editMode">
      <div class="flex">
        <div class="title">{{note.title}}</div>
        <div class="block_delete"><a href="#" @click="deleteNote"><i class='bx bx-x'></i></a></div>
      </div>
      <div class="flex">
        <div class="subtitle">{{note.text}}</div>
        <div class="block_correction"><a href="#" @click="switchEditMode"><i class='bx bxs-pencil'></i></a></div>
      </div>
    </div>
    <div v-if="editMode">
      <div class="flex">
        <textarea class="block_notes_input_zone_title" v-model="title">{{note.title}}</textarea>
        <div class="block_delete"><a href="#" @click="switchEditMode"><i class='bx bx-x'></i></a></div>
      </div>
      <div class="flex">
        <textarea class="block_notes_input_zone_subtitle" v-model="text">{{note.text}}</textarea>
        <div class="block_correction"><a href="#" @click="saveNote"><i class='bx bx-check'></i></a></div>
      </div>
    </div>
  </div>
</template>

<script>
import 'boxicons'
import Axios from "axios";

export default {
  name: "note",
  data(){
    return {
      editMode: false,
      title: '',
      text: '',
    }
  },
  props:{
    note:{
      default() {
        return{
          "id": 1, // Read Only
          "title": "Title of a note",
          "text": "Text of a note",
          "created_at": "2022-06-07 22:55:26", // Read Only
          "updated_at": "2022-06-07 22:55:26"  // Read Only
        }
      },
    }
  },
  methods:{
    deleteNote(){
      Axios.post('http://backend-project/app-1/note/'+ this.note.id +'/delete')
          .then(res => {
            if(res.data._status){
              this.$emit('after-delete')
            }
          })
    },
    onDeletePressed(){
      this.$emit('onDeletePressed', this.note.id)
    },
    correctioneNote(){
      Axios.post('http://backend-project/app-1/note/'+ id +'/create', {
        "title": this.title,
        "text": this.text
      })
    },
    switchEditMode(){
      this.title = this.note.title
      this.text = this.note.text
      this.editMode = !this.editMode
    },
    saveNote(){
      this.note.text = this.text
      this.note.title = this.title
      this.editMode = false
      this.$emit('onSavePressed', this.note)
    },
  }

}
</script>

<style scoped>
  .note{
    width: 250px;
    border: 1px solid #171717;
    border-radius: 3px;
    font-size: 20px;
    padding: 20px 15px;
    margin: 50px;
    background-color: rgba(26, 26, 26, 0.18);
  }
  .flex{
    display: flex;
  }
  .title{
    font-weight: bold;
    font-size: 24px;
    margin-bottom: 15px;
    flex: 1 1 auto;
  }
  .bx{
    color: #050505;
    font-size: 30px;
  }
  .bxs-pencil{
    font-size: 22px;
    position: relative;
    top: 15px;
    left: 8px;
  }
  .bx-check{
    font-size: 30px;
    position: relative;
    top: 15px;
    left: 15px;
  }
  .block_delete{
    position: relative;
    bottom: 15px;
    left: 13px;
  }
  .subtitle{
    flex: 1 1 auto;
  }


</style>