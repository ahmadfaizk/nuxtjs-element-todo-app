<template>
  <div>
    <el-row>
      <el-col>
        <h3>Edit Kategori</h3>
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
      id: null,
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
  created () {
    this.id = this.$route.query.id
    this.getCategory()
  },
  methods: {
    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.updateCategory()
        } else {
          return false
        }
      })
    },
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
