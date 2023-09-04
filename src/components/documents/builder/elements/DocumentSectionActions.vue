<template>
    <div>
        <a-icon
            v-if="editAction"
            type="edit"
            @click="editSection"
            class="ml-10"
        />
        <a-icon
            v-if="cloneAction"
            type="copy"
            @click="cloneSection"
            class="ml-10"
        />
        <a-icon
            v-if="deleteAction"
            type="delete"
            @click="deleteSection"
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
        editSection() {
            this.$emit("editSection");
        },
        deleteSection() {
            this.$emit("deleteSection");
        },
        cloneSection() {
            this.$emit("cloneSection");
        },
    },
};
</script>

<style lang="scss" scoped></style>
