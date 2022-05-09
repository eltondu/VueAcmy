<template>
  <div class="main-page-custom">
    <b-alert show>Selected Rows: {{ selected }}</b-alert>
    <b-col sm="7" md="6" class="my-1">
      <b-pagination
        v-model="currentPage"
        :total-rows="totalRows"
        :per-page="perPage"
        align="fill"
        size="sm"
        class="my-0"
      ></b-pagination>
    </b-col>
    <b-table
      striped
      hover
      :items="items"
      :fields="fields"
      :busy="isBusy"
      selectable
      @row-selected="onRowSelected"
      :select-mode="selectMode"
      :current-page="currentPage"
      :per-page="perPage"
    >
      <template #table-busy>
        <div class="text-center text-danger my-2">
          <b-spinner class="align-middle"></b-spinner>
          <strong>Loading...</strong>
        </div>
      </template>
      <template #cell(actions)="row">
        <b-button
          size="sm"
          @click="onRowSelected(row.item)"
          class="mr-1"
          variant="info"
        >
          Editar
        </b-button>
        <span class="separator-custom"> | </span>
        <b-button
          size="sm"
          @click="removeRow(row.item.id)"
          class="mr-1"
          variant="danger"
        >
          Excluir
        </b-button>
      </template>
    </b-table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      isBusy: true,
      fields: [        
        { key: "firstName", sortable: true },
        { key: "lastName", sortable: true },
        { key: "idBook", sortable: true },
        { key: "id", sortable: true },
        { key: "actions", label: "Actions" },
      ],
      items: [],
      selectMode: "single",
      selected: [],
      totalRows: 1,
      currentPage: 1,
      perPage: 10,
      sortBy: "firstName",
    };
  },
  methods: {
    onRowSelected(items) {
      this.selected = items;
    },
    removeRow(id) {
      this.items.splice(
        this.items.findIndex(function (i) {
          return i.id === id;
        }),
        1
      );
    },
  },
  mounted() {
    axios
      .get("https://fakerestapi.azurewebsites.net/api/v1/Authors")
      .then((res) => {
        var before = Date.now();
        var elapsed = Date.now() - before;
        setTimeout(() => {
          this.items = res.data;
          this.tableRows = res.data;
          this.isBusy = !this.isBusy;
          this.totalRows = this.items.length;
        }, elapsed);
      })
      .catch((error) => {
        console.error(error);
      });
  },
  created() {},
};
</script>

<style>
.sr-only {
  display: none !important;
}
.separator-custom {
  color: transparent;
}
</style>
