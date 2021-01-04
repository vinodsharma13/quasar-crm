<template>
  <q-page padding>
    <q-card class="my-card">
      <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md q-pa-md">
        <div class="">
          <q-input v-model="text" type="text" label="Label" />
        </div>
        <div>
          <q-btn label="Submit" type="submit" color="primary" />
          <q-btn
            label="Reset"
            type="reset"
            color="primary"
            flat
            class="q-ml-sm"
          />
        </div>
      </q-form>
    </q-card>
  </q-page>
</template>

<script>
import { onMounted, reactive } from "@vue/composition-api";
import axios from "axios";
export default {
  name: "Order",
  setup(props, { root }) {
    const RootUrl = "http://localhost:8000";

    const state = reactive({
      order: {
        order_item: [],
        order_date: null,
        customer_name: "",
        customer_gstno: "",
        customer_address: "",
        customer_contact_person: "",
        customer_email: "",
        customer_mobile: "",
        amc_period: "",
        payment_remark: "",
        is_billed: false,
        gst_percent: null,
        branch: null,
        order_type: null,
        owner: null,
        order_devloper: null,
        customer_designation: null,
      },
    });

    onMounted(() => {
      const orderId = root.$route.params.id;
      updateOrder(orderId);
    });

    //todo update the order
    const updateOrder = async (orderId) => {
      await axios.get(`${RootUrl}/crm/orders/${orderId}`).then((response) => {
        state.order = response.data;
      });
    };

    return {
      state,
      updateOrder,
    };
  },
};
</script>

<style lang="scss" scoped>
</style>