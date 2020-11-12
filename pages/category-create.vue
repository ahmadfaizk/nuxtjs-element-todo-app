<template>
  <div>
    <el-row>
      <el-col>
        <h3>Buat Kategori Baru</h3>
      </el-col>
    </el-row>
    <el-form ref="categoryForm" label-position="top" label-width="100px" :model="form" :rules="rules">
      <el-form-item label="Nama Kategori" prop="name">
        <el-input v-model="form.name" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('categoryForm')">
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
      },
      rules: {
        name: [
          { required: true, message: 'Nama harus diisi', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.createCategory()
        } else {
          return false
        }
      })
    },
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
