<template>
    <Table />
</template>

<script>
    import Table from '@/components/CRUDTable.vue';
    import Footer from '@/components/Footer.vue';

    export default {
        name: "AdminView",
        components: {
            Table,
            Footer
        }
    }
</script>