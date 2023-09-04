<template>
    <div>
        <div class="container">
            <a-upload
                list-type="picture-card"
                :show-upload-list="false"
                :before-upload="beforeUpload"
                @change="onChangeImage"
            >
                <div v-if="imageUrl" v-html="content"></div>
                <div v-else>
                    <a-icon :type="loading ? 'loading' : 'plus'" />
                    <div class="ant-upload-text">Upload</div>
                </div>
            </a-upload>
            <!-- 
            <a-button
                v-if="imageUrl"
                type="danger"
                ghost
                class="remove"
                shape="circle"
                @click="removeImage()"
            >
                <a-icon type="delete" />
            </a-button> -->
        </div>
    </div>
</template>

<script>
const getBase64 = (img, callback) => {
    const reader = new FileReader();
    reader.addEventListener("load", () => callback(reader.result));
    reader.readAsDataURL(img);
};

export default {
    props: {
        content: String,
    },
    components: {},
    data() {
        return {
            loading: false,
            imageUrl: "",
            imageBase64: null,
        };
    },
    mounted() {
        this.imageUrl = this.content;
    },
    methods: {
        updateElement() {
            this.$emit("updateElement", this.content);
        },
        removeImage() {
            this.imageUrl = "";
            this.content = "IMAGEM";
            this.$emit("updateElement", this.content);
        },
        onChangeImage(info) {
            if (this.isValidFile) {
                getBase64(info.file.originFileObj, (imageBase64) => {
                    this.imageBase64 = imageBase64;
                    this.imageUrl = imageBase64;
                    this.content = `<div class="image-element" style=""><img src="${imageBase64}" alt="img" style="max-width: 100%" /></div>`;

                    this.$emit("updateElement", this.content);
                });
            }
        },
        beforeUpload(file) {
            const isValidImage =
                file.type === "image/jpeg" ||
                file.type === "image/png" ||
                file.type === "image/svg+xml";
            if (!isValidImage) {
                this.$message.error("Este arquivo não é válido!");
                return;
            }
            const validSize = file.size / 1024 / 1024 < 2;
            if (!validSize) {
                this.$message.error("A imagem deve ter menos que 2MB!");
                return;
            }
            this.isValidFile = true;
            return isValidImage && validSize;
        },
        // updateElement() {
        // this.$emit("updateElement", this.content);
        //},
    },
};
</script>

<style lang="scss" scoped></style>
