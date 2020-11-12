<template>
  <div>
    <el-row type="flex" justify="space-between">
      <el-col :span="12">
        <h3>Ini Kategori</h3>
      </el-col>
      <el-col :span="4">
        <nuxt-link to="/category-create">
          <el-button type="primary">
            Tambah Kategori
          </el-button>
        </nuxt-link>
      </el-col>
    </el-row>
    <el-table :data="items" style="width: 100%">
      <el-table-column type="index" label="No." />
      <el-table-column prop="name" label="Nama Kategori" />
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
      await axios.get('/api/category')
        .then((response) => {
          this.items = response.data.data
        })
    },
    handleEdit (index) {
      const id = this.items[index]._id
      this.$router.push({ path: 'category-edit', query: { id } })
    },
    handleDelete (index) {
      this.$confirm('Apakah anda yakin akan mengapus. Lanjutkan?', 'Peringatan', {
        confirmButtonText: 'Oke',
        cancelButtonText: 'Batal',
        type: 'warning'
      }).then(() => {
        const id = this.items[index]._id
        this.deleteCategory(id)
      })
    },
    async deleteCategory (id) {
      await axios.delete('api/category/' + id)
        .then(() => {
          this.$notify({
            title: 'Sukses',
            message: 'Berhasil mengapus',
            type: 'success'
          })
          this.fetchData()
        })
    }
  }
}
</script>
