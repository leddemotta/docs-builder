<template>
    <div>
        <vue-editor
            v-model="content"
            placeholder="Digite o texto aqui"
            :editor-toolbar="customToolbar"
            @text-change="updateElement"
        ></vue-editor>
    </div>
</template>

<script>
import { VueEditor } from "vue2-editor";
export default {
    props: {
        content: String,
    },
    components: {
        VueEditor,
    },
    methods: {
        updateElement() {
            this.$emit("updateElement", this.content);
        },
    },
};
</script>

<style lang="scss" scoped></style>
