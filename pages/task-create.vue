<template>
  <div>
    <el-row>
      <el-col>
        <h3>Buat Task Baru</h3>
      </el-col>
    </el-row>
    <el-form label-position="top" label-width="100px" :model="form">
      <el-form-item label="Nama Task">
        <el-input v-model="form.name" />
      </el-form-item>
      <el-form-item label="Deskripsi">
        <el-input v-model="form.description" type="textarea" />
      </el-form-item>
      <el-form-item label="Tanggal">
        <el-date-picker v-model="form.date" type="date" placeholder="Pick tanggal" style="width: 100%" />
      </el-form-item>
      <el-form-item label="Kategori">
        <el-select v-model="form.category" placeholder="Pilih kategori" style="width: 100%">
          <el-option v-for="item in categories" :key="item._id" :label="item.name" :value="item._id" />
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="createTask()">
          Submit
        </el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      form: {
        name: '',
        description: '',
        date: '',
        category: ''
      },
      categories: []
    }
  },
  created () {
    this.getCategories()
  },
  methods: {
    async createTask () {
      await axios.post('api/task', this.form)
        .then((response) => {
          this.showSuccess()
          this.$router.push('task')
        })
    },
    async getCategories () {
      await axios.get('api/category')
        .then((response) => {
          this.categories = response.data.data
        })
    },
    showSuccess () {
      this.$notify({
        title: 'Sukses',
        message: 'Berhasil menyimpan task baru',
        type: 'success'
      })
    }
  }
}
</script>
