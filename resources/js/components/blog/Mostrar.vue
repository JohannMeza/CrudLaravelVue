<template>
  <div class="container">
    <div class="row">
      <div class="col-lg-12 mb-4">
        <router-link :to='{name: "crearBlog"}' class="btn btn-success"><i class="fas fa-plus-circle"></i></router-link>
      </div>

      <div class="col-12">
        <div class="table-responsive">
          <table class="table table-bordered">
            <thead class="bg-primary text-white">
              <tr>
                <th>ID</th>
                <th>Titulo</th>
                <th>Contenido</th>
                <th>Acciones</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="blog in blogs" :key="blog.id">
                <td>{{ blog.id }}</td>
                <td>{{ blog.titulo }}</td>
                <td>{{ blog.contenido }}</td>
                <td>
                  <router-link :to='{ name:"editarBlog", params: { id: blog.id } }' class="btn btn-info"><i class="fas fa-edit"></i></router-link>
                  <a type="button" @click="borrarBlog(blog.id)" class="btn btn-danger"><i class="fas fa-trash"></i></a>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "blogs",
  data() {
    return {
      blogs: []
    }
  },
  methods: {
    async mostrarBlogs () {
      await this.axios.get('/api/blog')
      .then(response => {
        this.blogs = response.data
      })
      .catch(error => {
        this.blog = []
      })
    },

    borrarBlog (id) {
      if (confirm("¿Confirma eliminar el registro?")) {
        this.axios.delete(`/api/blog/${id}`)
        .then(response => {
          this.mostrarBlogs()
        })
        .catch(error => {
          console.log(error)
        })
      }
    }
  },
  mounted () {
    this.mostrarBlogs()
  }
}
</script>     