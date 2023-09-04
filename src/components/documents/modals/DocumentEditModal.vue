<template>
    <div>
        <a-row :gutter="20">
            <a-col class="mb-20" :span="24">
                <a-checkbox
                    class="mb-20"
                    v-model="tempDocument.is_general"
                    style="
                        font-size: 12px;
                        font-weight: 500;
                        position: relative;
                    "
                >
                    Marque se este documento é para todas empresas
                </a-checkbox>
            </a-col>

            <a-col v-show="!tempDocument.is_general" :span="12">
                <a-form-item class="travel-input-outer">
                    <label :class="'filled'"> Empresa </label>

                    <a-select
                        class="travel-input"
                        placeholder="Selecione uma empresa"
                        optionFilterProp="txt"
                        @change="getCompanyBranchesOnChange"
                        v-decorator="[
                            `company_id`,
                            {
                                rules: [
                                    {
                                        required: true,
                                        message: 'Obrigatório',
                                    },
                                ],
                            },
                        ]"
                        show-search
                        style="width: 100%"
                    >
                        <a-select-option
                            v-for="(item, index) of companiesList"
                            :key="index"
                            :value="item.id"
                            :txt="item.trading_name"
                        >
                            {{ item.id }} -
                            {{ item.trading_name }}
                        </a-select-option>
                    </a-select>
                </a-form-item>
            </a-col>

            <a-col v-show="!tempDocument.is_general" :span="12">
                <a-form-item class="travel-input-outer">
                    <label :class="'filled'"> Filial </label>

                    <a-select
                        class="travel-input"
                        placeholder="Selecione uma filial"
                        optionFilterProp="txt"
                        :disabled="companyBranchesList.length == 0"
                        v-decorator="[
                            `company_branch_id`,
                            {
                                rules: [
                                    {
                                        required: true,
                                        message: 'Obrigatório',
                                    },
                                ],
                            },
                        ]"
                        show-search
                        style="width: 100%"
                    >
                        <a-select-option
                            v-for="(item, index) of companyBranchesList"
                            :key="index"
                            :value="item.id"
                            :txt="item.name"
                        >
                            {{ item.id }} -
                            {{ item.name }}
                        </a-select-option>
                    </a-select>
                </a-form-item>
            </a-col>
        </a-row>
    </div>
</template>

<script>
export default {
    props: {
        docForm: Object,
        tempDocument: Object,
    },
    data() {
        return {
            companiesList: [],
            companyBranchesList: [],
        };
    },
    mounted() {
        this.$http.get(`/company/list?page=1&per_page=100`).then(({ data }) => {
            this.companiesList = data.data;
        });

        if (this.tempDocument.company_id != 0) {
            this.getCompanyBranches(this.tempDocument.company_id);
        }

        this.docForm.setFieldsValue({
            company_id:
                this.tempDocument.company_id != 0
                    ? this.tempDocument.company_id
                    : undefined,
        });

        setTimeout(() => {
            this.docForm.setFieldsValue({
                company_branch_id:
                    this.tempDocument.company_branch_id != 0
                        ? this.tempDocument.company_branch_id
                        : undefined,
            });
        }, 20);
    },
    methods: {
        getCompanyBranchesOnChange(id) {
            this.docForm.setFieldsValue({
                [`company_branch_id`]: undefined,
            });

            this.companyBranchesList = [];
            this.getCompanyBranches(id);
        },
        getCompanyBranches(companyId) {
            this.$http
                .get(
                    `/company-branch/list?page=1&per_page=100&status=Ativo&company_id=${companyId}`
                )
                .then(({ data }) => {
                    this.companyBranchesList = data.data;
                })
                .catch(({ response }) => {
                    response;
                });
        },
    },
};
</script>

<style lang="sass" scoped></style>
