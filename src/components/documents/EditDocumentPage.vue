<!-- <template>
  <div class="document-builder pd-0">
    <a-form
      v-if="$store.state.userData.id == 1"
      :form="docForm"
      @submit="handleSubmit"
      @onFieldsChange="updateTempDocument"
    >
      <a-row>
        <a-col class="top-bar" :span="24">
          <a-row type="flex" justify="space-between">
            <a-col class="">
              <a-row type="flex" justify="start" :gutter="20">
                <a-col>
                  <a-icon
                    class="menu"
                    type="menu"
                    @click="openDocumentInfosDrawer = true"
                /></a-col>
                <a-col>
                  <a-form-item class="model-name">
                    <a-input
                      v-decorator="[
                        'name',
                        {
                          rules: [
                            {
                              required: true,
                              message: 'Obrigatório',
                            },
                          ],
                        },
                      ]"
                      placeholder="NOME DO MODELO"
                    >
                    </a-input> </a-form-item
                ></a-col>

                <a-col>
                  <a-form-item class="contract-id">
                    <a-input
                      v-decorator="['contract_id']"
                      placeholder="ID CONTRATO"
                      @blur="getContract"
                    >
                    </a-input> </a-form-item
                ></a-col>

                <a-col>
                  <a-icon
                    v-if="!previewDoc"
                    class="prev"
                    type="eye"
                    @click="previewDoc = true"
                  />

                  <a-icon
                    v-if="previewDoc"
                    class="prev"
                    type="edit"
                    @click="previewDoc = false"
                  />
                </a-col>

                <a-col>
               

                  <a-icon
                    v-if="previewDoc"
                    class="ml-10 pdf"
                    type="file-pdf"
                    @click="generatePDF(tempDocument.title)"
                  />
                </a-col>
              </a-row>
            </a-col>
            <a-col class="">
              <span class="status">
                <a-checkbox
                  class="mb-20"
                  v-model="tempDocument.status"
                  style=""
                >
                  {{ tempDocument.status ? "ATIVO" : "DESATIVADO" }}
                </a-checkbox>
              </span>
              <a-button
                html-type="submit"
                :loading="loadingUpdateButton"
                size="large"
                type="primary"
                >Atualizar</a-button
              >
            </a-col>
          </a-row>
        </a-col>

        <a-col class="builder-container" :span="24">
          <div id="print-area" class="builder-area">
            <div
              v-if="!previewDoc && document.structure.length > 0"
              class="collapse-all"
              @click="collapseAllPages"
            >
              RECOLHER TUDO
            </div>
            <draggable
              v-model="document.structure"
              group="page"
              @start="startDrag"
              @end="endDrag"
            >
              <a-row v-for="(page, index) in document.structure" :key="index">
                <a-col class="page" :span="24">
                  <a-col v-if="!previewDoc" class="page-header" :span="24">
                    <a-row type="flex" justify="space-between">
                      <a-col class="title">
                        <span
                          ><a-icon type="file-text" /> PAGINA {{ index + 1 }}
                        </span>

                        <span class="dotted-phrase">
                          {{ previewPageFirstContent(page, index) }}
                        </span>
                      </a-col>

                      <a-col class="">
                        <span class="ml-20 delete" @click="deletePage(index)"
                          ><a-icon type="delete"
                        /></span>

                        <span
                          class="ml-20 collapse-arrow"
                          @click="collapsePage(index)"
                          ><a-icon type="caret-down" v-if="page.collapse" />
                          <a-icon type="caret-up" v-if="!page.collapse"
                        /></span>
                      </a-col>
                    </a-row>
                  </a-col>
                  <a-col
                    v-if="page.collapse || previewDoc"
                    class="page-content"
                    :class="previewDoc ? 'prev-mode' : ''"
                    :span="24"
                    :style="`padding: ${page.css.padding}; font-size: ${page.css.fontSize};`"
                  >
                    <a-row>
                      <a-col class="sections" :span="24">
                        <draggable
                          v-model="page.sections"
                          group="sections"
                          @start="startDrag"
                          @end="endDrag"
                        >
                          <a-row
                            v-for="(section, sIndex) in page.sections"
                            :key="sIndex"
                          >
                            <DocumentOneColumnSection
                              v-if="section.type == 'col1'"
                              :elements="section.elements"
                              :section="section"
                              :pageData="page"
                              :pIndex="index"
                              :sIndex="sIndex"
                              @deleteSection="deleteSection"
                              @updateDocPages="updateDocPages"
                              @updateSectionElements="updateSectionElements"
                            />

                            <DocumentTwoColumnSection
                              v-if="section.type == 'col2'"
                              :pageData="page"
                              :pIndex="index"
                              :sIndex="sIndex"
                              :docPages="document.structure"
                              :columns="section.columns"
                              @deleteSection="deleteSection"
                              @updateDocPages="updateDocPages"
                            />
                          </a-row>
                        </draggable>
                      </a-col>

                      <a-col
                        v-if="!previewDoc"
                        class="add-thing"
                        @click="openInsertColumn(page, index)"
                        :span="24"
                      >
                        ADD COLUNAS
                      </a-col>
                    </a-row>

                    <div v-if="!previewDoc" class="security-line">
                      <span
                        >MARGEM DE SEGURANÇA PDF
                        <font class="f8"> (não passar a linha) </font></span
                      >
                    </div>
                  </a-col>
                </a-col>
                <a-col class="mb-20" :span="24">
              
                </a-col>
              </a-row>
            </draggable>

            <a-row>
              <a-col v-if="!previewDoc" class="add-page" :span="24">
                <a-button @click="addNewPage()">ADD PAGINA</a-button>
              </a-col>
            </a-row>

            <div id="live-debug" style="display: none">
              <json-viewer
                v-if="$store.state.userData.id == 1"
                class="mt-10 mb-20"
                :value="document.structure"
                :show-array-index="false"
                :expand-depth="4"
              ></json-viewer>
            </div>
          </div>
        </a-col>
      </a-row>

      <a-drawer
        placement="left"
        :closable="true"
        :width="$root.windowWidth > 960 ? '600px' : '100%'"
        :visible="openDocumentInfosDrawer"
        @close="openDocumentInfosDrawer = false"
      >
        <template slot="title"> DADOS DO DOCUMENTO </template>

        <DocumentEditModal
          v-if="openDocumentInfosDrawer"
          :docForm="docForm"
          :document="document.details"
          :tempDocument="tempDocument"
        />
      </a-drawer>

      <a-modal
        title="INSERIR COLUNAS"
        :footer="false"
        v-model="docBuilder.modals.openCollumns"
        @ok="docBuilder.modals.openCollumns = false"
        width="800px"
      >
        <DocumentInsertColumnModal
          :page="docBuilder.selected.page"
          @addSelectedColumn="addSelectedColumn"
        />
      </a-modal>
    </a-form>
  </div>
</template>

<script>
import html2pdf from "html2pdf.js";
import draggable from "vuedraggable";

import DocumentOneColumnSection from "../builder/sections/DocumentOneColumnSection.vue";
import DocumentTwoColumnSection from "../builder/sections/DocumentTwoColumnSection.vue";
import documentMixins from "../mixins/documentMixins";
import DocumentEditModal from "../modals/DocumentEditModal.vue";
import DocumentInsertColumnModal from "../modals/DocumentInsertColumnModal.vue";

export default {
  components: {
    draggable,
    DocumentInsertColumnModal,
    DocumentOneColumnSection,
    DocumentTwoColumnSection,
    DocumentEditModal,
  },
  mixins: [documentMixins],
  data() {
    return {
      docForm: this.$form.createForm(this, {
        onValuesChange: this.updateTempDocument,
      }),
      previewDoc: false,
      loadingUpdateButton: false,
      openDocumentInfosDrawer: false,
      generatePDFLoading: false,
      activateDoc: false,
      startDrag: false,
      endDrag: false,
      docBuilder: {
        modals: {
          openCollumns: false,
        },
        selected: {
          index: undefined,
          page: undefined,
        },
      },
    };
  },
  mounted() {
    this.getDocument(this.$route.params.id);
  },
  methods: {
    updateTempDocument(props, fields) {
      let field = Object.keys(fields)[0],
        value = Object.values(fields)[0];

      this.tempDocument[field] = value;

      if (Array.isArray(value)) {
        this.tempDocument[field] = JSON.stringify(value);
      }
    },
    getContract(id) {
      id;
    },
    updateSectionElements(pIndex, sIndex, newElements) {
      pIndex, sIndex, newElements;
      this.document.structure[pIndex].sections[sIndex].elements = newElements;
    },
    deletePage(pIndex) {
      this.document.structure.splice(pIndex, 1);
    },
    clonePage(pIndex) {
      let pageToClone = this.document.structure[pIndex];
      this.document.structure.splice(pIndex, 0, {
        collapse: pageToClone.collapse,
        sections: pageToClone.sections,
        css: pageToClone.css,
      });
    },
    previewPageFirstContent(page, index) {
      index;
      let theContent = "Nenhum elemento inserido";
      if (page.sections.length > 0) {
        if (page.sections[0].elements != undefined) {
          if (page.sections[0].elements.length > 0) {
            theContent = page.sections[0].elements[0].content;
          }
        }

        if (page.sections[0].columns != undefined) {
          if (page.sections[0].columns[0].elements.length > 0) {
            theContent = page.sections[0].columns[0].elements[0].content;
          }
        }
      }

      theContent = theContent.replace(/<\s*br\/*>/gi, "\n");
      theContent = theContent.replace(
        /<\s*a.*href="(.*?)".*>(.*?)<\/a>/gi,
        " $2 (Link->$1) "
      );
      theContent = theContent.replace(/<\s*\/*.+?>/gi, "\n");
      theContent = theContent.replace(/ {2,}/gi, " ");
      theContent = theContent.replace(/\n+\s*/gi, "\n\n");

      return theContent;
    },
    updateDocPages(pageIndex, sectionIndex, newSection) {
      this.document.structure[pageIndex].sections[sectionIndex] = newSection;
    },
    deleteSection(pIndex, sIndex) {
      console.log("deleteSection", pIndex, sIndex);
      this.document.structure[pIndex].sections.splice(sIndex, 1);
    },
    collapseAllPages() {
      this.document.structure.forEach((page) => {
        page.collapse = false;
      });
    },
    collapsePage(index) {
      if (this.document.structure[index].collapse) {
        this.document.structure[index].collapse = false;
      } else {
        this.document.structure[index].collapse = true;
      }
    },
    addNewPage() {
      this.document.structure.push({
        collapse: true,
        css: {
          padding: "20px",
        },
        sections: [],
      });
    },
    addSelectedColumn(type) {
      this.docBuilder.modals.openCollumns = false;

      let updateDocPages = this.document.structure;

      if (type === "col1") {
        updateDocPages[this.docBuilder.selected.index].sections.push({
          type: "col1",
          elements: [],
        });
      }

      if (type === "col2") {
        updateDocPages[this.docBuilder.selected.index].sections.push({
          type: "col2",
          columns: [
            {
              elements: [],
            },
            {
              elements: [],
            },
          ],
        });
      }

      this.document.structure = updateDocPages;
    },
    openInsertColumn(page, index) {
      this.docBuilder.selected.page = page;
      this.docBuilder.selected.index = index;
      this.docBuilder.modals.openCollumns = true;
    },
    generatePDF(docName) {
      this.generatePDFLoading = true;

      let element = document.getElementById("print-area");
      let options = {
        filename: docName,
        // this.contractId > 0
        //     ? `${docName} - ${this.contract.meta.client_name.toUpperCase()}.pdf`
        //     : this.document.title,
        image: { type: "jpeg", quality: 0.3 },
        enableLinks: false,
        margin: [0, 0, 0, 0],
        html2canvas: { scale: 2, useCORS: true },
        jsPDF: {
          format: "a4",
          orientation: "portrait",
        },
        pagebreak: { mode: "avoid-all", after: ".avoidThisRow" },
      };

      // if (this.contrato.pagamento.credito.ativo) {
      //     if (
      //         this.contract.meta[
      //             "payment_methods_credit_card_client_name_1"
      //         ] &&
      //         this.contract.meta[
      //             "payment_methods_credit_card_client_month_1"
      //         ] &&
      //         this.contract.meta[
      //             "payment_methods_credit_card_client_year_1"
      //         ]
      //     ) {
      //         html2pdf().from(element).set(options).save();
      //         this.generatePDFLoading = false;
      //     } else {
      //         this.showPasswordModal = true;
      //         this.$message.warning(
      //             "Você está gerando o PDF sem exibir os dados de cartão do cliente. Insira sua senha para exibir os dados."
      //         );
      //         this.generatePDFLoading = false;
      //     }
      // } else {
      html2pdf().from(element).set(options).save();

      this.generatePDFLoading = false;
      //  }
    },
    printDocs(docName) {
      // if (this.contractId > 0) {
      //     document.title = `${docName} - ${this.contract.meta.client_name.toUpperCase()}`;
      // } else {
      //     document.title = `${this.document.title}`;
      // }

      docName;

      // if (this.contrato.pagamento.credito.ativo) {
      //     if (
      //         this.contract.meta[
      //             "payment_methods_credit_card_client_name_1"
      //         ] &&
      //         this.contract.meta[
      //             "payment_methods_credit_card_client_month_1"
      //         ] &&
      //         this.contract.meta[
      //             "payment_methods_credit_card_client_year_1"
      //         ]
      //     ) {
      //         window.print();
      //     } else {
      //         this.showPasswordModal = true;
      //         this.$message.warning(
      //             "Você está prestes a imprimir o documento sem exibir os dados de cartão do cliente. Insira sua senha para exibir os dados."
      //         );
      //     }
      // } else {
      //     window.print();
      // }

      window.print();
    },
    handleSubmit(e) {
      e.preventDefault();
      this.docForm.validateFields((err, values) => {
        values;
        if (!err) {
          this.loadingUpdateButton = true;

          this.tempDocument.html = this.document.structure;
          this.tempDocument.modified_by = {
            name: `${this.$store.state.userData.first_name} ${this.$store.state.userData.last_name}`,
            id: this.$store.state.userData.id,
          };

          this.$http
            .post("/document/update", this.tempDocument)
            .then(({ data }) => {
              this.$message.success(data.message);
            })
            .catch(({ response }) => {
              this.$message.error(response.data.message);
            })
            .finally(() => {
              this.loadingUpdateButton = false;
            });
        }
      });
    },
  },
};
</script>

<style lang="sass">
.builder-area
  .prev-mode
    height: 1062px !important
    .outer-box
      .add-thing, .empty-space
        display: none !important
    .content
      background: #FFF
    .box-elements
      border: 0
      padding: 0
</style>

<style>
@media print {
  body * {
    visibility: hidden;
  }
  @page {
    size: auto;
    margin: 20px 20px;
  }

  #embed-pdf,
  .travel-header.ant-page-header,
  header.ant-layout-header,
  .sidebar {
    display: none !important;
  }

  #the-outer-document,
  #the-outer-document * {
    visibility: visible;
  }

  .sidebar.ant-col.ant-col-6 {
    position: absolute;
  }

  .contract.view {
    background: #fff !important;
  }

  .dashboard .ant-layout-content {
    margin: 0 !important;
    position: absolute;
    left: 0;
    padding: 0 !important;
  }

  .document-viewer {
    width: 100%;
    position: absolute;
    left: 0;
    top: 0;
  }

  .ant-row {
    position: absolute !important;
    left: 0 !important;
  }

  .ant-tabs {
    position: absolute;
    left: 0;
    top: 0;
  }

  #the-outer-document {
    position: absolute;
    left: 0;
    top: 0;
  }

  .ant-page-header-ghost {
    position: absolute;
  }

  .contract.view .contract-table {
    box-shadow: none;
  }

  #contrato {
    display: none;
  }

  .contract.view .contract-table .a4 {
    height: 1006px !important;
  }

  .contract.view .contract-table .a4 .td {
    height: 1016px !important;
  }

  .document-viewer {
    padding: 0px !important;
    background: transparent;
    height: 100%;
    overflow: initial;
    box-shadow: none;
    border-radius: 0;
    border: 0;
  }

  @media only screen and (min-device-width: 1601px) and (max-width: 3000px) {
    #pg-print {
      height: auto !important;
    }
  }
}
</style>

<style lang="sass" scoped>
.document-builder
  .top-bar
    position: sticky
    top: 0
    z-index: 10
    box-shadow: 0 0 40px #ddd
    padding: 10px
    background: #fff
    .pdf
      position: relative
      top: 6px
      font-size: 28px
      color: #E74C3C
    .prev
      position: relative
      top: 6px
      font-size: 28px
      color: #999
    .menu
      position: relative
      top: 6px
      font-size: 28px
      color: #bbb
      margin-left: 10px
    .ant-form-item
      margin: 0
    .contract-id
      input
        border: 0
        width: 120px
        background: #eee
    .model-name
      input
        border: 0
        width: 600px
        background: #eee
    .status
      .ant-checkbox-wrapper.ant-checkbox-wrapper-checked
        border: 1px solid #ccc
        color: #be4178
      .ant-checkbox-wrapper
        border: 1px solid #eee
        padding: 10px
        border-radius: 6px
        margin: 0 10px 0 0 !important
  .builder-container
    padding: 30px 30px 30px
    .collapse-all
      cursor: pointer
      position: sticky
      top: 63px
      margin-bottom: 2px
      z-index: 10
      width: 90px
      font-size: 9px
      text-align: center
      float: right
      background: #434a53
      font-weight: 600
      padding: 5px
      color: #FFF
      opacity: 0.2
      transition: .3s
    .collapse-all:hover
      opacity: 1
    .builder-area
      margin: 0 auto
      width: 802px
      .add-page
        position: relative
        z-index: 10
        text-align: center
        padding: 10px
        transition: .3s
      .page
        position: relative
        .page-header
          position: relative
          z-index: 1
          background: #434a53
          font-weight: 500
          padding: 10px
          color: #FFF
          width: 802px
          //width: 990px
          .title
            font-size: 13px
            .dotted-phrase
              width: 300px
              display: inline-block
              opacity: 0.2
              font-size: 10px
              position: relative
              top: 3px
              left: 70px
          .collapse-arrow, .css, .delete
            cursor: pointer
            font-size: 13px
        .page-content
          position: relative
          overflow: hidden
          width: 802px
          height: 1332px
          // width: 990px
          // height: 1402px
          background: #FFF
          box-shadow: 0 0 90px #cbcbcb

          .security-line
            position: absolute
            border-top: 2px dashed #999
            left: 0
            right: 0
            bottom: 0
            padding: 3px 20px
            font-size: 10px
            background: #FFF
            opacity: 0.8
            z-index: 1
            color: #999
            font-weight: 500
            padding-bottom: 100px
          .add-thing
            text-align: center
            padding: 8px 14px
            border: 2px solid #ddd
            cursor: pointer
            color: #aaa
            margin: 5px 0
            font-weight: 500
</style> -->
