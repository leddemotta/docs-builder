<template>
    <div>
        <div class="element-wrapper text">
            <div class="content">
                <div v-html="content" />
            </div>
            <DocumentElementActions
                :editAction="true"
                :cloneAction="true"
                :deleteAction="true"
                @editElement="editElement"
                @deleteElement="deleteElement"
                @cloneElement="cloneElement"
                class="actions"
            />
        </div>
        <a-modal
            title="HTML"
            :footer="false"
            v-model="openEdit"
            @ok="openEdit = false"
            width="800px"
        >
            <DocumentHtmlElementEdit
                @updateElement="updateElement"
                :content="content"
            />
        </a-modal>
    </div>
</template>

<script>
import DocumentElementActions from "./DocumentElementActions.vue";
import DocumentHtmlElementEdit from "./DocumentHtmlElementEdit.vue";
export default {
    props: {
        content: String,
        cIndex: Number,
        eIndex: Number,
    },
    components: { DocumentElementActions, DocumentHtmlElementEdit },
    data() {
        return {
            openEdit: false,
        };
    },
    methods: {
        editElement() {
            this.openEdit = true;
        },
        cloneElement() {
            this.$emit("cloneElement", this.eIndex, this.cIndex);
        },
        deleteElement() {
            this.$emit("deleteElement", this.eIndex, this.cIndex);
        },
        updateElement(newContent) {
            this.content = newContent;
            this.$emit("updateElement", this.eIndex, this.cIndex, newContent);
        },
    },
};
</script>

<style lang="sass" scoped>
.element-wrapper:hover
  .actions
    display: block
.element-wrapper
  position: relative
  .content
    cursor: grab
    background: #efeff0
    clear: both
    display: block
    float: left
    width: 100%
    margin-bottom: 1px
  .actions
    position: absolute
    background: #fff
    font-size: 14px
    padding: 0 5px
    right: 0
    top: 0
    display: none
</style>
