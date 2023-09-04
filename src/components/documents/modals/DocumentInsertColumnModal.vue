<template>
    <div>
        <a-row class="columns" :gutter="20">
            <a-col :span="6">
                <div class="option one-col" @click="selectCollumn('col1')">
                    <a-row>
                        <a-col :span="24">
                            <div class="col col1"></div>
                        </a-col>
                    </a-row>
                </div>
            </a-col>

            <a-col :span="6">
                <div class="option two-col" @click="selectCollumn('col2')">
                    <a-row :gutter="5">
                        <a-col :span="12"> <div class="col col1"></div> </a-col>
                        <a-col :span="12"> <div class="col col2"></div> </a-col>
                    </a-row>
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
        selectCollumn(selected) {
            this.$emit("addSelectedColumn", selected);
        },
    },
};
</script>

<style lang="sass" scoped>
.columns
  .option
    display: block
    width: 100%
    height: 100px
    background: #fff
    border: 1px solid #ddd
    cursor: pointer
    padding: 5px
    .col
      width:100%
      height:89px
      background: #ddd
</style>
