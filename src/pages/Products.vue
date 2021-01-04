<template>
  <q-page padding>
    <q-table
      title="Products"
      :data="state.data"
      :columns="columns"
      row-key="id"
      :pagination.sync="state.pagination"
      :rows-per-page-options="[2]"
      @request="onRequest"
    />
  </q-page>
</template>

<script>
import { onMounted, reactive } from "@vue/composition-api";
import Axios from "axios";
export default {
  setup(props, { root }) {
    const RootUrl = "http://localhost:8000";

    const state = reactive({
      data: [],
      pagination: {
        page: 1,
        rowsNumber: null,
      },
    });

    function routeToEdit(event, row, index) {
      console.log(row.id);
      root.$router.push({ path: `/products/${row.id}` });
    }

    const columns = [
      {
        name: "idt",
        label: "IDENTITY",
        field: "id",
      },
      {
        name: "name",
        label: "PRODUCT NAME",
        field: "name",
      },
    ];

    onMounted(() => {
      onRequest();
    });

    const onRequest = (prop) => {
      Axios.get(
        prop
          ? RootUrl + `/crm/products/?page=${prop.pagination.page}`
          : RootUrl + "/crm/products/"
      ).then((response) => {
        prop
          ? (state.pagination.page = prop.pagination.page)
          : (state.pagination.page = 1);
        state.data = response.data.results;
        state.pagination.rowsNumber = response.data.count;
      });
      console.log(prop);
    };

    return {
      columns,
      state,
      onRequest,
    };
  },
};
</script>

<style lang="scss" scoped>
</style>