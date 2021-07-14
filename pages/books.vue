<template>
    <div>
        <NavBar />
        <EditBookModal :book="selectedBook" @onCancel="getAll"/>
        <DeleteBookModal :book="selectedBook" @onDeleted="getAll" />

        <div class="container">
            <h1>
                Books
                <BookEntryModal class="float-right" @onAdd="getAll" />
            </h1>

            <table class="table custom-table">
                <thead>
                    <tr class="bg-info text-white">
                        <th>Title</th>
                        <th>Description</th>
                        <th>Genre</th>
                        <th>Author</th>
                        <th>Date Published</th>
                        <th>&nbsp;</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="book in books" :key="book.id" class="bg-light">
                        <td>{{ book.title }}</td>
                        <td :title="book.description">
                            {{ book.description }}
                        </td>
                        <td>{{ book.genre }}</td>
                        <td>{{ book.author }}</td>
                        <td>{{ book.pub_date }}</td>
                        <td>
                            <button class="btn btn-info btn-sm mr-2 mb-1" @click="onEdit(book)">
                                <i class="fas fa-edit"></i>
                            </button>
                            <button class="btn btn-danger btn-sm mb-1" @click="onDelete(book)">
                                <i class="fas fa-trash"></i>
                            </button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            books: [],
            selectedBook: {}
        }
    },
    methods:{
        async getAll() {
            await this.$axios.get('/api/books')
            .then((res) => {
                if(res.status==200) {
                    this.books = res.data
                }
            })
        },
        onEdit(selectedBook) {
            this.selectedBook = selectedBook
            this.$bvModal.show('editBookModal')
        },
        onDelete(selectedBook) {
            this.selectedBook = selectedBook
            this.$bvModal.show('deleteBookModal')
        }
    },
    created() {
        this.getAll()
    }
}
</script>

<style scoped>
</style>