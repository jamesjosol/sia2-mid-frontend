<template>
    <div>
        <b-modal id="NoticeMsgModalAdd" :title="noticeTitle" :header-bg-variant="titleVariant" ok-only size="sm" centered ok-title="OK" ok-variant="success" button-size="sm" headerClass="p-2 border-bottom-0" footerClass="p-2 mt-2 border-top-0">
            {{ noticeMsg }}
        </b-modal>  

        <b-button v-b-modal.bookEntryModal variant="primary"><i class="fa fa-book"></i><i class="fa fa-plus small-icon"></i> Add Book</b-button>

        <b-modal id="bookEntryModal" title="Book Entry" ok-title="Save Book" @ok="onSubmit">
            <form action="#">
                <b-form-group
                    label="Title"
                    label-for="title"
                >
                <b-form-input id="title" v-model="book.title" trim></b-form-input>
                </b-form-group>

                <b-form-group
                    label="Description"
                    label-for="description"
                >
                <b-form-textarea id="description" v-model="book.description" trim rows="4" cols="50"> </b-form-textarea>
                </b-form-group>

                <b-form-group
                    label="Genre"
                    label-for="genre"
                >
                <b-form-select id="genre" v-model="book.genre" :options="genres"></b-form-select>
                </b-form-group>

                <b-form-group
                    label="Author"
                    label-for="author"
                >
                <b-form-input id="author" v-model="book.author" trim></b-form-input>
                </b-form-group>

                <b-form-group
                    label="Date Published"
                    label-for="pub_date"
                >
                <b-form-input type="date" id="pub_date" v-model="book.pub_date" trim></b-form-input>
                </b-form-group>
            </form>
        </b-modal>
    </div>
</template>

<script>
export default {
    data() {
        return {
            book: {},   
            genres: [
                {value: 'Art', text: 'Art'},
                {value: 'Anthologies', text: 'Anthologies'},
                {value: 'Biography', text: 'Biography'},
                {value: 'Comics', text: 'Comics'},
                {value: 'Fantasy', text: 'Fantasy'},
                {value: 'Fiction', text: 'Fiction'},
                {value: 'History', text: 'History'},
                {value: 'Horror', text: 'Horror'},
                {value: 'Music', text: 'Music'},
                {value: 'Mystery', text: 'Mystery'},
                {value: 'Romance', text: 'Romance'},
                {value: 'Science', text: 'Science'},
                {value: 'Thriller', text: 'Thriller'},
            ],
            noticeMsg: '',
            noticeTitle: '',
            titleVariant: '',
        }
    },
    methods: {
        async onSubmit() {
            this.$axios.post('/api/books', this.book)
            .then((res) => {
                if(res.status==202) {

                    this.$bvModal.show('NoticeMsgModalAdd')
                    this.noticeMsg = 'Book successfully added.'
                    this.noticeTitle = "Notification"
                    this.titleVariant = "success"
                    setTimeout(() => {
                        this.$bvModal.hide('NoticeMsgModalAdd')
                    }, 2 * 1000)
                    this.$emit('onAdd')
                    this.book = {}
                }
            })
            .catch((err) => {
                this.noticeMsg = err.response.data.message
                this.noticeTitle = "Error!"
                this.titleVariant = "danger"
                this.$bvModal.show('NoticeMsgModalAdd')
                setTimeout(() => {
                    this.$bvModal.hide('NoticeMsgModalAdd')
                }, 2 * 1000)

            })
        }
    }
}
</script>

<style scoped>
.small-icon{
    font-size: 0.7em;
    margin-left: -2px;
}
</style>