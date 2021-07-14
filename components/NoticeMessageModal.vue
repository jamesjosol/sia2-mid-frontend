<template>
    <b-modal id="NoticeMsgModal" :title="title" ok-only size="sm" centered ok-title="OK" ok-variant="success" button-size="sm" headerClass="p-2 border-bottom-0" footerClass="p-2 mt-2 border-top-0">
        {{ message }}
    </b-modal>
</template>

<script>
export default {
    props: {
        message: '',
        title: '',
    },
}
</script>