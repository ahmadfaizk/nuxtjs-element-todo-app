<template>
  <div>
    <el-row>
      <el-col>
        <h3>Buat Kategori Baru</h3>
      </el-col>
    </el-row>
    <el-form label-position="top" label-width="100px" :model="form">
      <el-form-item label="Nama Kategori">
        <el-input v-model="form.name" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="createCategory()">
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
        name: ''
      }
    }
  },
  created () {
  },
  methods: {
    async createCategory () {
      await axios.post('api/category', this.form)
        .then((response) => {
          this.showSuccess()
          this.$router.push('category')
        })
    },
    showSuccess () {
      this.$notify({
        title: 'Sukses',
        message: 'Berhasil menyimpan kategori baru',
        type: 'success'
      })
    }
  }
}
</script>
