<template>
    <div>
        <a-icon
            v-if="editAction"
            type="edit"
            @click="editElement"
            class="ml-10"
        />
        <a-icon
            v-if="cloneAction"
            type="copy"
            @click="cloneElement"
            class="ml-10"
        />
        <a-icon
            v-if="deleteAction"
            type="delete"
            @click="deleteElement"
            class="ml-10"
        />
    </div>
</template>

<script>
export default {
    props: {
        editAction: Boolean,
        cloneAction: Boolean,
        deleteAction: Boolean,
    },
    methods: {
        editElement() {
            this.$emit("editElement");
        },
        deleteElement() {
            this.$emit("deleteElement");
        },
        cloneElement() {
            this.$emit("cloneElement");
        },
    },
};
</script>

<style lang="scss" scoped></style>
