<template>
    <div>
        <a-row class="columns" :gutter="[10, 10]">
            <a-col :span="24"> <b>COMUNS</b> </a-col>
            <a-col :span="8">
                <div class="option" @click="selectElement('text')">TEXTO</div>
            </a-col>

            <a-col :span="8">
                <div class="option" @click="selectElement('title')">TÍTULO</div>
            </a-col>

            <a-col :span="8">
                <div class="option" @click="selectElement('image')">IMAGEM</div>
            </a-col>

            <a-col :span="8">
                <div class="option" @click="selectElement('html')">HTML</div>
            </a-col>
        </a-row>

        <a-row class="mt-30 columns" :gutter="[10, 10]">
            <a-col :span="24"> <b>CONTRATOS</b> </a-col>
            <a-col :span="8">
                <div class="option" @click="selectElement('customerTable')">
                    TABELA CONTRATANTE
                </div>
            </a-col>

            <a-col :span="8">
                <div class="option" @click="selectElement('paymentTable')">
                    TABELA PAGAMENTOS
                </div>
            </a-col>
        </a-row>
    </div>
</template>

<script>
export default {
    props: {},
    data() {
        return {};
    },
    methods: {
        selectElement(selected) {
            if (selected === "text") {
                this.$emit("addSelectedElement", {
                    type: "text",
                    content: "Alterar texto",
                    css: {},
                });
            }

            if (selected === "title") {
                this.$emit("addSelectedElement", {
                    type: "title",
                    content: "Alterar título",
                    css: {},
                });
            }

            if (selected === "image") {
                this.$emit("addSelectedElement", {
                    type: "image",
                    content: "IMAGEM",
                    css: {},
                });
            }

            if (selected === "html") {
                this.$emit("addSelectedElement", {
                    type: "html",
                    content: "<div>Insira o HTML<div>",
                    css: {},
                });
            }

            // this.$emit("addSelectedElement", {});
        },
    },
};
</script>

<style lang="sass" scoped>
.columns
  .option
    display: block
    width: 100%
    background: #fff
    border: 1px solid #ddd
    cursor: pointer
    font-weight: 600
    padding: 15px 10px
    text-align: center
</style>
