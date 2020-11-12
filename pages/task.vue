<template>
  <div>
    <el-row type="flex" justify="space-between">
      <el-col :span="12">
        <h3>Ini Task</h3>
      </el-col>
      <el-col :span="4">
        <nuxt-link to="task-create">
          <el-button type="primary">
            Tambah Task
          </el-button>
        </nuxt-link>
      </el-col>
    </el-row>
    <el-table :data="items" style="width: 100%">
      <el-table-column type="index" label="No." />
      <el-table-column prop="name" label="Nama Task" />
      <el-table-column prop="category.name" label="Kategori" />
      <el-table-column prop="date" :formatter="dateFormatter" label="Tanggal" />
      <el-table-column prop="done" :formatter="statusFormatter" label="Status" />
      <el-table-column label="Operasi">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.$index)">
            Edit
          </el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.$index)">
            Delete
          </el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      items: []
    }
  },
  created () {
    this.fetchData()
  },
  methods: {
    async fetchData () {
      await axios.get('api/task')
        .then((response) => {
          this.items = response.data.data
        })
    },
    handleEdit (index) {
      const id = this.items[index]._id
      this.$router.push({ path: 'task-edit', query: { id } })
    },
    handleDelete (index) {
      this.$confirm('Apakah anda yakin akan mengapus. Lanjutkan?', 'Peringatan', {
        confirmButtonText: 'Oke',
        cancelButtonText: 'Batal',
        type: 'warning'
      }).then(() => {
        const id = this.items[index]._id
        this.deleteTask(id)
      })
    },
    async deleteTask (id) {
      await axios.delete('api/task/' + id)
        .then(() => {
          this.$notify({
            title: 'Sukses',
            message: 'Berhasil mengapus data',
            type: 'success'
          })
          this.fetchData()
        })
    },
    statusFormatter (row, column) {
      if (row.done) {
        return 'Selesai'
      } else {
        return 'Belum Selesai'
      }
    },
    dateFormatter (row, column) {
      return new Date(row.date).toLocaleDateString('id', {
        weekday: 'long',
        day: 'numeric',
        month: 'short',
        year: 'numeric'
      })
    }
  }
}
</script>
