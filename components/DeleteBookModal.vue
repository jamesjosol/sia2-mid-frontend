<template>
    <div>
        <b-modal id="NoticeMsgModalDelete" :title="noticeTitle" ok-only size="sm" centered ok-title="OK" ok-variant="success" button-size="sm" headerClass="p-2 border-bottom-0" footerClass="p-2 mt-2 border-top-0">
            {{ noticeMsg }}
        </b-modal> 

        <b-modal id="deleteBookModal" title="Delete Book" ok-title="Delete Book" ok-variant="danger" footerClass="p-2 mt-2 border-top-0" @ok="onDelete">
            Are you sure you want to delete this book? <br>
            <h5>{{ book.title }}</h5>
        </b-modal>
    </div>
</template>

<script>
export default {
    props: {
        book: {}
    },
    data() {
        return {
            noticeMsg: '',
            noticeTitle: ''
        }
    },
    methods: {
        async onDelete() {
            this.$axios.delete('/api/books/' + this.book.id)
            .then((res) => {
                if(res.status==202) {
                    //alert('Delete Successful')
                    this.$bvModal.show('NoticeMsgModalDelete')
                    this.noticeMsg = this.book.title + " deleted successfully."
                    this.noticeTitle = "Notification"
                    setTimeout(() => {
                        this.$bvModal.hide('NoticeMsgModalDelete')
                    }, 2 * 1000)
                    this.$emit('onDeleted')
                }
            })
            .catch((err) => {
                alert(err.message)
            })
        }
    }
}
</script>