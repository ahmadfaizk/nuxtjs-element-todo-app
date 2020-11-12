<template>
  <div>
    <el-row>
      <el-col>
        <h3>Edit Task</h3>
      </el-col>
    </el-row>
    <el-form ref="taskForm" label-position="top" label-width="100px" :model="form" :rules="rules">
      <el-form-item label="Nama Task" prop="name">
        <el-input v-model="form.name" />
      </el-form-item>
      <el-form-item label="Deskripsi" prop="description">
        <el-input v-model="form.description" type="textarea" />
      </el-form-item>
      <el-form-item label="Tanggal" prop="date">
        <el-date-picker v-model="form.date" type="date" placeholder="Pick tanggal" style="width: 100%" />
      </el-form-item>
      <el-form-item label="Kategori" prop="category">
        <el-select v-model="form.category" placeholder="Pilih kategori" style="width: 100%">
          <el-option v-for="item in categories" :key="item._id" :label="item.name" :value="item._id" />
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-checkbox v-model="form.done">
          Selesai
        </el-checkbox>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('taskForm')">
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
        name: '',
        description: '',
        date: '',
        category: '',
        done: false
      },
      categories: [],
      rules: {
        name: [
          { required: true, message: 'Nama harus diisi', trigger: 'blur' }
        ],
        description: [
          { required: true, message: 'Deskripsi harus diisi', trigger: 'blur' }
        ],
        date: [
          { required: true, message: 'Tanggal harus diisi', trigger: 'change' }
        ],
        category: [
          { required: true, message: 'Kategori harus dipilih', trigger: 'change' }
        ]
      }
    }
  },
  created () {
    this.getCategories()
    this.id = this.$route.query.id
    this.getTask()
  },
  methods: {
    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.updateTask()
        } else {
          return false
        }
      })
    },
    async updateTask () {
      await axios.put('api/task/' + this.id, this.form)
        .then((response) => {
          this.showSuccess()
          this.$router.push('task')
        })
    },
    async getTask () {
      await axios.get('api/task/' + this.id)
        .then((response) => {
          this.form = response.data.data
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
        message: 'Berhasil mengubah task',
        type: 'success'
      })
    }
  }
}
</script>
