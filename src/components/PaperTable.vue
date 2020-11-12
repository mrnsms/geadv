<template>
  <div id="app">
    <b-container fluid>
      <!-- User Interface controls -->
      <b-row>
        <b-col lg="6" class="my-1">
          <b-form-group
            label="Filtro"
            label-cols-sm="3"
            label-align-sm="right"
            label-size="sm"
            label-for="filterInput"
            class="mb-0"
          >
            <b-input-group size="sm">
              <b-form-input
                v-model="filter"
                type="search"
                id="filterInput"
                placeholder="Localizar"
              ></b-form-input>
              <b-input-group-append>
                <b-button :disabled="!filter" @click="filter = ''"
                  >Limpar</b-button
                >
              </b-input-group-append>
            </b-input-group>
          </b-form-group>
        </b-col>
        <b-col sm="5" md="6" class="my-1">
          <b-form-group
            label="Por página"
            label-cols-sm="6"
            label-cols-md="4"
            label-cols-lg="3"
            label-align-sm="right"
            label-size="sm"
            label-for="perPageSelect"
            class="mb-0"
          >
            <b-form-select
              v-model="perPage"
              id="perPageSelect"
              size="sm"
              :options="pageOptions"
            ></b-form-select>
          </b-form-group>
        </b-col>
      </b-row>

      <b-table
        stripped
        hover
        empty-text="Sem dados para apresentar"
        small
        stacked="md"
        foot-clone
        :items="items"
        :fields="fields"
        :current-page="currentPage"
        :per-page="perPage"
        :filter="filter"
        :filter-included-fields="filterOn"
        :sort-desc.sync="sortDesc"
        :sort-direction="sortDirection"
        :sort-by.sync="sortBy"
        @filtered="onFiltered"
      >
      </b-table>

      <div class="justify-content-center row my-1">
        <b-pagination
          size="md"
          :total-rows="this.items.length"
          :per-page="perPage"
          v-model="currentPage"
        />
      </div>
    </b-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      fields: [
        {
          key: "D2N",
          label: "Variável",
          sortable: true,
          sortByFormatted: true,
          filterByFormatted: true,
        },
        {
          key: "D3C",
          label: "Mês",
          sortable: true,
          sortByFormatted: true,
          filterByFormatted: true,
        },
        {
          key: "D4N",
          label: "Grupo/SubGrupo",
          sortable: true,
          sortByFormatted: true,
          filterByFormatted: true,
        },
        {
          key: "V",
          label: "Valor",
          sortable: true,
          sortByFormatted: true,
          filterByFormatted: true,
        },
      ],
      totalRows: 1,
      currentPage: 1,
      perPage: 20,

      pageOptions: [20, 40, 80],
      sortBy: "D3C",
      sortDesc: true,
      sortDirection: "asc",
      filter: null,
      filterOn: [],
      infoModal: {
        id: "info-modal",
        title: "",
        content: "",
      },
    };
  },
  created() {
    let promise = this.$http.get("http://api-psi-geadv.herokuapp.com/");
    promise.then((res) => {
      res.json().then((items) => (this.items = items));
    });
  },
  computed: {
    sortOptions() {
      // Create an options list from our fields
      return this.fields
        .filter((f) => f.sortable)
        .map((f) => {
          return { text: f.label, value: f.key };
        });
    },
  },
  mounted() {
    // Set the initial number of items
    this.totalRows = this.items.length;
  },
  methods: {
    info(item, index, button) {
      this.infoModal.title = `Row index: ${index}`;
      this.infoModal.content = JSON.stringify(item, null, 2);
      this.$root.$emit("bv::show::modal", this.infoModal.id, button);
    },
    resetInfoModal() {
      this.infoModal.title = "";
      this.infoModal.content = "";
    },
    onFiltered(filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering
      this.totalRows = filteredItems.length;
      this.currentPage = 1;
    },
  },
};
</script>