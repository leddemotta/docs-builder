<template>
    <div>
        <a-row>
            <a-col class="box-elements" :span="24">
                <a-row>
                    <a-col class="actions a-right" :span="24">
                        <DocumentSectionActions
                            :editAction="false"
                            :cloneAction="false"
                            :deleteAction="true"
                            @editSection="editSection"
                            @deleteSection="deleteSection"
                            @cloneSection="cloneSection"
                        />
                    </a-col>
                </a-row>

                <a-row>
                    <a-col :span="24">
                        <draggable
                            v-model="elements"
                            group="elements"
                            @start="startDrag"
                            @end="endDrag"
                        >
                            <a-row
                                v-for="(element, eIndex) in elements"
                                :key="eIndex"
                            >
                                <DocumentTextElement
                                    v-if="element.type == 'text'"
                                    :content="
                                        element.content
                                            ? element.content
                                            : '...'
                                    "
                                    :eIndex="eIndex"
                                    @updateElement="updateElement"
                                    @deleteElement="deleteElement"
                                    @cloneElement="cloneElement"
                                />

                                <DocumentImageElement
                                    v-if="element.type == 'image'"
                                    :content="
                                        element.content
                                            ? element.content
                                            : '...'
                                    "
                                    :eIndex="eIndex"
                                    @updateElement="updateElement"
                                    @deleteElement="deleteElement"
                                    @cloneElement="cloneElement"
                                />

                                <DocumentHtmlElement
                                    v-if="element.type == 'html'"
                                    :content="
                                        element.content
                                            ? element.content
                                            : '...'
                                    "
                                    :eIndex="eIndex"
                                    @updateElement="updateElement"
                                    @deleteElement="deleteElement"
                                    @cloneElement="cloneElement"
                                />
                            </a-row>

                            <a-row v-if="elements.length == 0">
                                <a-col
                                    class="empty-space"
                                    :span="24"
                                    @click="openElementsModal = true"
                                >
                                    ADD OU ARRASTE
                                </a-col>
                            </a-row>
                        </draggable>
                    </a-col>

                    <a-col
                        class="add-thing"
                        @click="openElementsModal = true"
                        :span="24"
                        v-if="elements.length > 0"
                    >
                        ADD ELEMENTO
                    </a-col>
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
import DocumentSectionActions from "../elements/DocumentSectionActions.vue";
import DocumentInsertElementModal from "../../modals/DocumentInsertElementModal.vue";

import DocumentHtmlElement from "../elements/DocumentHtmlElement.vue";
import DocumentTextElement from "../elements/DocumentTextElement.vue";
import DocumentImageElement from "../elements/DocumentImageElement.vue";

export default {
    props: {
        pageData: Object,
        elements: Array,
        section: Object,
        pIndex: Number,
        sIndex: Number,
    },
    data() {
        return {
            startDrag: false,
            openElementsModal: false,
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
        endDrag() {
            console.log("end drag", this.elements);
            this.$emit(
                "updateSectionElements",
                this.pIndex,
                this.sIndex,
                this.elements
            );
        },
        addSelectedElement(elementObj) {
            this.elements.push(elementObj);
            this.openElementsModal = false;
        },
        editSection() {},
        deleteSection() {
            this.$emit("deleteSection", this.pIndex, this.sIndex);
        },
        cloneSection() {},
        updateElement(eIndex, cIndex, newContent) {
            cIndex;
            this.elements[eIndex].content = newContent;
        },
        deleteElement(eIndex, cIndex) {
            cIndex;
            this.elements.splice(eIndex, 1);
        },
        cloneElement(eIndex, cIndex) {
            cIndex;
            let elementToClone = this.elements[eIndex];
            this.elements.splice(eIndex, 0, {
                type: elementToClone.type,
                content: elementToClone.content,
                css: elementToClone.css,
            });
        },
    },
};
</script>

<style lang="sass" scoped>
.box-elements
  .actions, .add-thing
    display: none
.box-elements:hover
  .actions, .add-thing
    display: block !important
.box-elements
  border: 1px solid #ddd
  padding: 5px 5px 5px
  margin: 0 0 10px
  .empty-space
    text-align: center
    padding: 8px 14px
    border: 2px dashed #ddd
    cursor: pointer
    color: #aaa
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
