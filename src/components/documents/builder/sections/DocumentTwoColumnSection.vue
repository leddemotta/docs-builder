<template>
    <div>
        <a-row class="box-elements outer-box mb-10">
            <a-col class="a-right actions" :span="24">
                <DocumentSectionActions
                    :editAction="false"
                    :cloneAction="false"
                    :deleteAction="true"
                    @editSection="editSection"
                    @deleteSection="deleteSection"
                    @cloneSection="cloneSection"
                />
            </a-col>

            <a-col :span="24">
                <a-row :gutter="20">
                    <draggable
                        v-model="columns"
                        group="columns"
                        @start="startDrag"
                        @end="endDrag"
                    >
                        <a-col
                            v-for="(column, cIndex) in columns"
                            :key="cIndex"
                            :span="12"
                        >
                            <a-row>
                                <a-col class="box-elements" :span="24">
                                    <a-row>
                                        <a-col :span="24">
                                            <draggable
                                                v-model="column.elements"
                                                group="elements"
                                                @start="startDrag"
                                                @end="endDrag"
                                            >
                                                <a-row
                                                    v-for="(
                                                        element, eIndex
                                                    ) in column.elements"
                                                    :key="eIndex"
                                                >
                                                    <DocumentTextElement
                                                        v-if="
                                                            element.type ==
                                                            'text'
                                                        "
                                                        :content="
                                                            element.content
                                                                ? element.content
                                                                : '...'
                                                        "
                                                        :cIndex="cIndex"
                                                        :eIndex="eIndex"
                                                        @updateElement="
                                                            updateElement
                                                        "
                                                        @deleteElement="
                                                            deleteElement
                                                        "
                                                        @cloneElement="
                                                            cloneElement
                                                        "
                                                    />

                                                    <DocumentImageElement
                                                        v-if="
                                                            element.type ==
                                                            'image'
                                                        "
                                                        :content="
                                                            element.content
                                                                ? element.content
                                                                : '...'
                                                        "
                                                        :cIndex="cIndex"
                                                        :eIndex="eIndex"
                                                        @updateElement="
                                                            updateElement
                                                        "
                                                        @deleteElement="
                                                            deleteElement
                                                        "
                                                        @cloneElement="
                                                            cloneElement
                                                        "
                                                    />

                                                    <DocumentHtmlElement
                                                        v-if="
                                                            element.type ==
                                                            'html'
                                                        "
                                                        :content="
                                                            element.content
                                                                ? element.content
                                                                : '...'
                                                        "
                                                        :cIndex="cIndex"
                                                        :eIndex="eIndex"
                                                        @updateElement="
                                                            updateElement
                                                        "
                                                        @deleteElement="
                                                            deleteElement
                                                        "
                                                        @cloneElement="
                                                            cloneElement
                                                        "
                                                    />
                                                </a-row>

                                                <a-row
                                                    v-if="
                                                        column.elements
                                                            .length == 0
                                                    "
                                                >
                                                    <a-col
                                                        class="empty-space"
                                                        :span="24"
                                                        @click="
                                                            onClickOpenElementsModal(
                                                                cIndex
                                                            )
                                                        "
                                                    >
                                                        ADD OU ARRASTE
                                                    </a-col>
                                                </a-row>
                                            </draggable>
                                        </a-col>
                                        <a-col
                                            v-if="column.elements.length > 0"
                                            @click="
                                                onClickOpenElementsModal(cIndex)
                                            "
                                            class="add-thing"
                                            :span="24"
                                        >
                                            ADD ELEMENTO
                                        </a-col>
                                    </a-row>
                                </a-col>
                            </a-row>
                        </a-col>
                    </draggable>
                </a-row>
            </a-col>
        </a-row>

        <a-modal
            title="INSERIR ELEMENTOS"
            :footer="false"
            v-model="openElementsModal"
            @ok="openElementsModal = false"
            width="800px"
        >
            <DocumentInsertElementModal
                @addSelectedElement="addSelectedElement"
            />
        </a-modal>
    </div>
</template>

<script>
import draggable from "vuedraggable";
import DocumentInsertElementModal from "../../modals/DocumentInsertElementModal.vue";
import DocumentHtmlElement from "../elements/DocumentHtmlElement.vue";
import DocumentImageElement from "../elements/DocumentImageElement.vue";
import DocumentSectionActions from "../elements/DocumentSectionActions.vue";
import DocumentTextElement from "../elements/DocumentTextElement.vue";

export default {
    props: {
        columns: Array,
        section: Object,
        sIndex: Number,
        pIndex: Number,
    },
    data() {
        return {
            startDrag: false,
            endDrag: false,
            openElementsModal: false,
            selectedColumn: undefined,
        };
    },
    components: {
        draggable,
        DocumentTextElement,
        DocumentInsertElementModal,
        DocumentSectionActions,
        DocumentHtmlElement,
        DocumentImageElement,
    },
    methods: {
        deleteSection() {
            this.$emit("deleteSection", this.pIndex, this.sIndex);
        },
        cloneSection() {},
        editSection() {},
        onClickOpenElementsModal(cIndex) {
            this.openElementsModal = true;
            this.selectedColumn = cIndex;
        },
        addSelectedElement(elementObj) {
            this.columns[this.selectedColumn].elements.push(elementObj);
            this.openElementsModal = false;
            this.selectedColumn = undefined;
        },
        updateElement(eIndex, cIndex, newContent) {
            this.columns[cIndex].elements[eIndex].content = newContent;
        },
        deleteElement(eIndex, cIndex) {
            this.columns[cIndex].elements.splice(eIndex, 1);
        },
        cloneElement(eIndex, cIndex) {
            let elementToClone = this.columns[cIndex].elements[eIndex];
            this.columns[cIndex].elements.splice(eIndex, 0, {
                type: elementToClone.type,
                content: elementToClone.content,
                css: elementToClone.css,
            });
        },
    },
};
</script>

<style lang="sass" scoped>
.outer-box
  .actions, .add-thing
    display: none
.outer-box:hover
  .actions, .add-thing
    display: block !important
.box-elements
  border: 1px solid #ddd
  padding: 5px 5px 5px

  .empty-space
    text-align: center
    padding: 8px 14px
    border: 2px dashed #eee
    cursor: pointer
    color: #ddd
    margin: 5px 0
    font-weight: 500
  .add-thing
    text-align: center
    padding: 8px 14px
    border: 2px solid #ddd
    cursor: pointer
    color: #aaa
    margin: 5px 0
    font-weight: 500
</style>
