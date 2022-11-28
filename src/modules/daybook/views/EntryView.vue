<template>
    <div v-if="entrada"
    class="entry-title d-flex justify-content-between p-2">
          <div>
              <span class="text-success fs-3 fw-bold">{{day}}</span>
              <span class="mx-1 fs-3">{{month}}</span>
              <span class="mx-2 fs-4 fw-light"> {{yearDay}}</span>
          </div>
  
          <div>
              <button class="btn btn-danger mx-2">
                  Borrar
                  <i class="fa fa-trash-alt"></i>
              </button>
  
              <button class="btn btn-primary">
                  Subir foto
                  <i class="fa fa-upload"></i>
              </button>
  
          </div>
    </div>
  
          <hr>
          <div v-if="entrada"
          class="d-flex flex-column px-3 h-75">
              <textarea 
              v-model="entrada.text"
              placeholder="What happened today?">
              </textarea>
          </div>
  
          <Fab icon="fa-save"/>
          <img src="https://www.seiu1000.org/sites/main/files/main-images/camera_lense_0.jpeg" alt="entry-picture"  class="img-thumbnail">
  
  </template>
  
  <script>


  import { defineAsyncComponent } from 'vue'
  import { mapGetters } from 'vuex';
  import getDayMonthYear from '../helpers/getDayMonthYear'


  
  
  export default {

data(){
    return{
        entrada: null
    }
},
 

    props:{
        id:{
            type:String,
            required: true
        }
    },

        components:{
            Fab: defineAsyncComponent(() => import("../components/Fab.vue")),
        },



        computed:{
            ...mapGetters('journal', ['getEntryById']),

            day(){
                    const {day} = getDayMonthYear(this.entrada.date)
                    return day
            },

            month(){
                const {month} = getDayMonthYear(this.entrada.date)
                return month
            },

            yearDay(){
                const {yearDay} = getDayMonthYear(this.entrada.date)
                return yearDay
            }
                },



        methods:{
            loadEntry(){
              const entry = this.getEntryById(this.id)
              console.log(entry)
              if(!entry) return this.$router.push({name:'no-entry'})
              this.entrada = entry
            }
        },

        created(){
            // console.log(this.$route.params.id)
            this.loadEntry()
        },


        watch: {
            id(){
                this.loadEntry()
            }
        }

    
  }



  </script>
  
  <style lang="scss" scoped>
  
  textarea{
      font-size: 20px;
      border: none;
      height: 100%;
      &:focus{
          outline: none;
      }
  }
  
  img{
      width: 200px;
      position: fixed;
      bottom: 150px;
      right: 20px;
      box-shadow: 0pc 5px 10px rgb(black, $alpha:0.2);
  }
  
  </style>