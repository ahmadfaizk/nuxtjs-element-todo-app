<template>
  <div>
    <el-row>
      <el-col>
        <h3>Edit Kategori</h3>
      </el-col>
    </el-row>
    <el-form label-position="top" label-width="100px" :model="form">
      <el-form-item label="Nama Kategori">
        <el-input v-model="form.name" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="updateCategory()">
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
      id: null,
      form: {
        name: ''
      }
    }
  },
  created () {
    this.id = this.$route.query.id
    this.getCategory()
  },
  methods: {
    async getCategory () {
      await axios.get('api/category/' + this.id).then((response) => {
        this.form = response.data.data
      })
    },
    async updateCategory () {
      await axios.put('api/category/' + this.id, this.form)
        .then((response) => {
          this.showSuccess()
          this.$router.push('category')
        })
    },
    showSuccess () {
      this.$notify({
        title: 'Sukses',
        message: 'Berhasil mengubah kategori',
        type: 'success'
      })
    }
  }
}
</script>
