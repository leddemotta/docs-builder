<template>
    <div>
        <a-textarea
            v-model="content"
            placeholder="Insira o código"
            :auto-size="{ minRows: 20, maxRows: 20 }"
            @change="updateElement"
        />
    </div>
</template>

<script>
export default {
    props: {
        content: String,
    },
    components: {},
    methods: {
        updateElement() {
            this.$emit("updateElement", this.content);
        },
    },
};
</script>

<style lang="scss" scoped></style>
