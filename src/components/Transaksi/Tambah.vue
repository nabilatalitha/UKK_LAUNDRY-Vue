<template>
<div id="wrapper">
 <sidebar-component></sidebar-component>
<div id="content-wrapper" class="d-flex flex-column">
<div id="content">
<navbar-component></navbar-component>
<div class="container-fluid">
<div class="row">
<div class="col-lg-8">
<div class="card shadow mb-4">
<div class="card-header py-3">
<h6 class="m-0 font-weight-bold text-primary">Tambah Transaksi</h6>
</div>
<div class="card-body">
<form @submit.prevent="tambah">
<div class="form-group">
<label>Member</label>
<select v-model="transaksi.id_member" class="form-control">
<option v-for="(m, index) in member" :key="index" :value="m.id_member">
{{ m.nama_member }}
</option>
</select>
</div>
<button type="submit" class="btn btn-success btn-block">Simpan</button>
</form>
</div>
</div>
</div>
</div>
</div>
</div>
<footer-component></footer-component>
</div>
</div>
</template>

<script>
export default {
    data() {
        return {
            member : {},
            transaksi : {}
        }
    },
    created() {
        var data = JSON.parse(this.$store.state.datauser)
        var role = data.role

        if(role == 'owner')
        {
            this.$swal("Error","Anda tidak dapat mengakses halaman ini","error")
            this.$router.push('/')
        }
    this.axios.get('http://localhost:8000/api/member', {headers : { Authorization : 'Bearer ' + this.$store.state.token}} )
    .then( (res) => {
        console.log(res.data)
        this.member = res.data.data
    })
    .catch(err => console.log(err))
    },
    methods : {
    tambah() {
        this.axios.post('http://localhost:8000/api/transaksi',
        this.transaksi,
        {headers : {Authorization : 'Bearer ' + this.$store.state.token}} )
        .then( (res) => {
             if(res.data.success) {
                this.$swal("Sukses", res.data.message, "success")
                this.$router.push('/transaksi');
             }
        })
        .catch(err => console.log(err))
    }
    }
}
</script>