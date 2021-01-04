<template>
  <q-page>
    <q-table
      title="Orders"
      :loading="loading"
      :data="state.somedata"
      :pagination.sync="state.pagination"
      :columns="columns"
      row-key="id"
      :rows-per-page-options="[2]"
      @request="onRequest"
      @row-dblclick="routeToEdit"
    />
  </q-page>
</template>

<script>
import axios from "axios";
import {
  computed,
  onBeforeMount,
  onMounted,
  reactive,
  ref,
} from "@vue/composition-api";

export default {
  setup(props, { root }) {
    const RootUrl = "http://localhost:8000";

    const state = reactive({
      somedata: [],
      pagination: {
        page: 1,
        rowsNumber: null,
      },
    });

    function routeToEdit(event, row, index) {
      console.log(row.id);
      root.$router.push({ path: `/order/${row.id}` });
    }

    const loading = ref(false);

    const columns = [
      {
        name: "order_no",
        label: "order_no",
        field: "order_no",
        // sortable: true,
      },
      {
        name: "order_date",
        label: "order_date",
        field: "order_date",
        // sortable: true,
      },
      {
        name: "customer_name",
        label: "customer_name",
        field: "customer_name",
        // sortable: true,
      },
      {
        name: "customer_mobile",
        label: "customer_mobile",
        field: "customer_mobile",
        // sortable: true,
      },
      {
        name: "gst_percent",
        label: "gst_percent",
        field: "gst_percent",
        // sortable: true,
      },
      {
        name: "gst_amount",
        label: "gst_amount",
        field: "gst_amount",
        // sortable: true,
      },
      {
        name: "amount",
        label: "amount",
        field: "amount",
        // sortable: true,
      },
    ];

    onMounted(async () => {
      console.log(RootUrl);
      onRequest();
    });

    async function onRequest(props) {
      loading.value = true;
      await axios
        .get(
          props
            ? `${RootUrl}/crm/orders?page=${props.pagination.page}`
            : `${RootUrl}/crm/orders/`
        )
        .then((response) => {
          state.somedata = response.data.results;
          state.pagination.rowsNumber = response.data.count;
          props
            ? (state.pagination.page = props.pagination.page)
            : (state.pagination.page = 1);
        })
        .catch((error) => console.log("Error", error.message));
      loading.value = false;
    }

    return {
      columns,
      state,
      loading,
      onRequest,
      routeToEdit,
    };
  },
};
</script>

<style lang="sass">
.my-sticky-header-column-table
  /* height or max-height is important */
  // height: 310px
  /* specifying max-width so the example can
   highlight the sticky column on any browser window */
  max-width: 100%
  td:first-child
    /* bg color is important for td; just specify one */
    background-color: #c1f4cd !important
  tr th
    position: sticky
    /* higher than z-index for td below */
    z-index: 2
    /* bg color is important; just specify one */
    background: #fff
  /* this will be the loading indicator */
  thead tr:last-child th
    /* height of all previous header rows */
    top: 48px
    /* highest z-index */
    z-index: 3
  thead tr:first-child th
    top: 0
    z-index: 1
  tr:first-child th:first-child
    /* highest z-index */
    z-index: 3
  td:first-child
    z-index: 1
  td:first-child, th:first-child
    position: sticky
    left: 0
</style>
