<template>
  <b-container fluid>
    <!-- UI controls -->
    <b-row class="my-4">
      <b-col sm="12" md="6" class="m-2">
        <b-form-group
          label="Filter"
          label-for="filter-input"
          label-cols-sm="3"
          label-align-sm="right"
          label-size="md"
          class="mb-0"
        >
          <b-input-group size="md">
            <b-form-input
              id="filter-input"
              v-model="filter"
              type="search"
              placeholder="Type to Search"
            ></b-form-input>

            <b-input-group-append>
              <b-button :disabled="!filter" @click="filter = ''"
                >Clear</b-button
              >
            </b-input-group-append>
          </b-input-group>
        </b-form-group>
      </b-col>
      <b-col sm="12" md="2" class="m-2">
        <b-form-select
          v-model="rowCountSelected"
          @change="updateRecordList"
          class="mb-3"
        >
          <b-form-select-option-group label="Select Table Rows">
            <b-form-select-option :value="null" disabled
              >-- Table Rows --</b-form-select-option
            >

            <b-form-select-option :value="5">5</b-form-select-option>
            <b-form-select-option :value="10">10</b-form-select-option>
            <b-form-select-option :value="20">20</b-form-select-option>
            <b-form-select-option :value="50">50</b-form-select-option>
            <b-form-select-option :value="100">100</b-form-select-option>
            <b-form-select-option :value="200">200</b-form-select-option>
          </b-form-select-option-group>
        </b-form-select>
      </b-col>
      <b-col sm="12" md="2" class="m-2 ml-auto">
        <b-button v-if="!editMode" @click="onEdit" variant="outline-primary"
          >Edit Table</b-button
        >
        <b-button v-if="editMode" @click="onCancel" variant="outline-danger"
          >Cancel</b-button
        >
      </b-col>
    </b-row>

    <!-- Main table element -->
    <b-row>
      <b-col sm="10" class="mx-auto">
        <b-table
          hover
          sort-icon-left
          bordered
          borderless
          striped
          head-variant="light"
          :items="items"
          :fields="fields"
          :current-page="currentPage"
          :per-page="perPage"
          :filter="filter"
          :sort-by.sync="sortBy"
          :sort-desc.sync="sortDesc"
          :sort-direction="sortDirection"
          stacked="md"
          show-empty
          small
          @filtered="onFiltered"
        >
          <template #cell()="row">
            <div class="py-2">
              {{ row.value }}
            </div>
          </template>
        </b-table>
      </b-col>
    </b-row>

    <!-- Pagination -->
    <b-row>
      <b-col sm="8" md="6" class="my-1 mx-auto">
        <b-pagination
          v-model="currentPage"
          :total-rows="totalRows"
          :per-page="perPage"
          align="fill"
          size="md"
          class="my-4"
        ></b-pagination>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  props: ["items"],
  data() {
    return {
      fields: [
        {
          key: "id",
          label: "id",
          sortable: true,
          sortDirection: "desc",
        },
        {
          key: "name",
          label: "name",
          sortable: true,
          class: "text-left",
        },
        {
          key: "email",
          label: "email",
          sortable: true,
          class: "text-left",
        },

        {
          key: "body",
          label: "body",
          sortable: true,
          class: "text-left",
        },

        // { key: "actions", label: "Actions" },
      ],
      isBusy: false,
      editMode: false,
      rowCountSelected: null,
      currentPage: 1,
      perPage: 5,
      totalRows : 1,
      pageOptions: [5, 10, 15, { value: 100, text: "Show a lot" }],
      sortBy: "",
      sortDesc: false,
      sortDirection: "asc",
      filter: null,
    };
  },

  mounted() {
    this.totalRows = this.items.length
  },

  methods: {

    onFiltered(filteredItems) {
      this.totalRows = filteredItems.length;
      this.currentPage = 1;
    },
    onEdit() {
      this.editMode = true;
      const trList = this.$children[2].$refs["item-rows"];
      trList.forEach((element) => {
        element.$el.contentEditable = true;
      });
    },
    onCancel() {
      this.editMode = false;
      const trList = this.$children[2].$refs["item-rows"];
      trList.forEach((element) => {
        element.$el.contentEditable = false;
      });
    },
    updateRecordList() {
      this.perPage = this.rowCountSelected;
    },
  },
};
</script>