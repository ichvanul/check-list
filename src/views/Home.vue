<template>
    <div class="container">
        <div class="box-get">
            <div class="label">
                <h3>Table</h3>
                <a href="#" @click="showModal">Add New To-Do-List</a>
            </div>
           <div class="head-tb">
               <div class="name-th">
                   <h4>To-Do-List</h4>
               </div>
               <div class="check-th">
                   <h4>Action</h4>
               </div>
           </div>

           <div class="head-tb tabel"
           v-for="data in datas[0]"
           :key="data.id"
           >
               <div class="name-th t-bg">
                   <h4>{{ data.name }}</h4>
               </div>
               <div class="check-th t-bg">
                   <button @click="deleteItem(data)">Delete</button>
               </div>
           </div>
        </div>
        <div class="modal-add">
            <form @submit.prevent="addItem" class="box-modal">
                <div class="ttl">
                    <h3>Add New To-Do-List</h3>
                    <h3 @click="closeModal">X</h3>
                </div>
                <p>To-Do-List</p>
                <input type="text" placeholder="name" v-model="nameItem">
                <button type="submit">Save</button>
            </form>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Home',
  data () {
    return {
      datas: [],
      nameItem: ''
    }
  },
  methods: {
    deleteItem (data) {
      const JwtToken = localStorage.getItem('token')
      axios.defaults.headers.common.Authorization = `Bearer ${JwtToken}`
      axios.delete(`http://18.141.178.15:8080/checklist/${data.id}`).then((result) => {
        if (result.data.statusCode === 2300) {
          alert('Data Deleted!')
          this.$router.go()
        }
      })
    },
    addItem () {
      const JwtToken = localStorage.getItem('token')
      axios.defaults.headers.common.Authorization = `Bearer ${JwtToken}`
      axios.post('http://18.141.178.15:8080/checklist', {
        name: this.nameItem
      }).then((result) => {
        if (result.data.statusCode === 2000) {
          alert('New Data Saved!')
          this.$router.go()
          document.querySelector('.modal-add').classList.remove('showModal')
        }
      })
    },
    showModal () {
      document.querySelector('.modal-add').classList.add('showModal')
    },
    closeModal () {
      document.querySelector('.modal-add').classList.remove('showModal')
    }
  },
  mounted () {
    if (!localStorage.token) {
      this.$router.push('/login')
    }
    const JwtToken = localStorage.getItem('token')
    axios.defaults.headers.common.Authorization = `Bearer ${JwtToken}`
    axios.get('http://18.141.178.15:8080/checklist').then((result) => {
      if (result.data.statusCode === 2100) {
        this.datas.push(result.data.data)
      }
    })
    console.log(this.datas)
  }
}
</script>

<style scoped>
.ttl{
  width: 100%;
  display: flex;
  justify-content: space-between;
}

.ttl h3{
  cursor: pointer;
}

.container{
  width: 80vw;
  display: flex;
  margin: 0 auto;
  background-color: white;
  justify-content: center;
  position: relative;
}

.box-get{
  width: 80%;
  display: flex;
  padding: 10px;
  flex-direction: column;
}

.label{
  width: 100%;
  height: 30px;
  justify-content: space-between;
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  outline: none;
}

.label a{
  background-color: gray;
  color: #fff;
  padding: 7px 15px;
  border-radius: 5px;
  outline: none;
  text-decoration: none;
}

.box-get .label h3{
  margin: 10px 0;
}

.head-tb{
  width: 100%;
  display: flex;
}

.name-th{
  flex: 3;
  display: flex;
  align-items: center;
  background-color: rgb(167, 167, 167);
  padding: 10px;
}

.check-th{
  flex: 1;
  display: flex;
  align-items: center;
  background-color: rgb(167, 167, 167);
  padding: 10px;
}

.check-th button{
  padding: 5px 10px;
  border: none;
  background-color: rgb(179, 46, 46);
  color: #fff;
  border-radius: 5px;
}

.t-bg{
  background-color: #fff!important;
  border-bottom: 1px solid #cacaac;
}

.showModal{
  display: flex!important
}

.modal-add{
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, .3);
  position: absolute;
  display: none;
  justify-content: center;
  align-items: center;
}

.box-modal{
  width: 40%;
  padding: 10px;
  display: flex;
  background-color: #fff;
  flex-direction: column;
}

.box-modal h3{
  margin-bottom: 10px;
}

.box-modal p{
  font-weight: bold;
}

.box-modal input{
  height: 20px;
  padding-left: 10px;
  border: 2px solid #cacaca;
  border-radius: 5px;
  margin: 10px 0;
}

.box-modal button{
  width: 100px;
  height: 30px;
  border: 1px solid #cacaca;
  background-color: rgba(0, 0, 0, .3);
  color: #fff;
  cursor: pointer;
}
</style>
