<template>
<div class="card shadow mt-3">
  <div class="card-body">
    <h5 class="card-title">Edit Friends</h5>
     <form class="row g-3" @submit.prevent="update">
  <div class="col-md-6">
    <label for="inputEmail4" class="form-label">Nama</label>
    <input type="text" class="form-control" id="inputEmail4" 
    v-model="friend.nama" />
      <div class="alert alert-danger" v-if="validation.nama">
        {{ validation.nama[0] }}
      </div>
  </div>
  <div class="col-md-6">
    <label for="inputPassword4" class="form-label">No Tlp</label>
    <input type="number" class="form-control" id="inputPassword4"
    v-model="friend.no_tno_telp"/>
    <div class="alert alert-danger" v-if="validation.no_tno_telp">
        {{ validation.no_tno_telp[0] }}
      </div>
  </div>
  <div class="col-6">
    <label for="inputAddress" class="form-label">Alamat</label>
    <input type="text" class="form-control" id="inputAddress" placeholder="Masukkan Alamat"
    v-model="friend.alamat" />
    <div class="alert alert-danger" v-if="validation.alamat">
        {{ validation.alamat[0] }}
      </div>
  </div>
  <div class="col-6">
    <label for="inputAddress" class="form-label">Group</label>
    <select class="form-select" aria-label="Default select example" v-model="friend.groups_id">
        <option v-for="group in groups" :key="group.id" :value="group.id">
          {{ group.name }}
          </option>
    </select>
  </div>
  
  <div class="col-12">
    <button type="submit" class="btn btn-primary">Edit</button>
  </div>
</form>
  </div>
</div>
 
</template>
<script>
import { onMounted, ref } from 'vue';
import { reactive } from 'vue';
import { useRouter, useRoute } from 'vue-router'
import axios from 'axios'
export default {
  setup() {
    
    const friend = reactive({
      nama: '',
      no_telp: '',
      alamat: '',
      groups_id: ''
    })
    
    let groups = ref([]);
    const validation = ref([]);
    
    const router = useRouter();
    
    const route = useRoute()

    onMounted(()=>{
      axios.get(`http://127.0.0.1:8000/api/friends/${route.params.id}/edit`)
      .then(response => {
        console.log(response.data.data.nama)

        friend.nama = response.data.data.nama
        friend.no_tno_telp = response.data.data.no_tno_telp
        friend.alamat = response.data.data.alamat
        friend.groups_id = response.data.data.groups_id
      }).catch(error =>{
        console.log(error.response.data)
      })

      axios.get('http://127.0.0.1:8000/api/groups')
      .then((response) => {
        groups.value = response.data.data;
        console.log(response);
      })
      .catch((error) => {
        validation.value = error.response.data;
        console.log(error);
      });
    })

    function update(){
      let nama = friend.nama
      letno_telp = friend.no_tno_telp
      let alamat = friend.alamat
      let groups_id = friend.groups_id

      axios.put(`http://127.0.0.1:8000/api/friends/${route.params.id}`, {
        nama: nama,
        no_telp:no_telp,
        alamat: alamat,
        groups_id: groups_id
      })
      .then(() => {
        router.push({
          name:'Home'
        })
      }).catch(error => {
        console.log(error)
      })
    }
    return {
      friend,
      validation,
      router, 
      update,
      route,
      groups
    }
  },
}
</script>