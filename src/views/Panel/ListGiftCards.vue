<template>
  <div>
    <div class="row">
      <div class="fixed-action-btn">
        <router-link
          class="btn-floating btn-large"
          :to="{ name: 'addGiftCard' }"
        >
          <i class="large material-icons">add</i>
        </router-link>
      </div>

      <!-- search box -->
      <form class="col s3 right" @submit.prevent="search">
        <div class="input-field">
          <i class="material-icons prefix">search</i>
          <input type="text" placeholder="search" v-model.lazy="searchtext" />
          <label for="icon_prefix2"></label>
        </div>
      </form>
    </div>

    <Loading v-if="isLoading" loadingMessage="Retriving gift cards" />

    <DataTable
      v-if="!isLoading"
      :header-fields="headerFields"
      :data="tableData || []"
      :css="tableStyles"
    >
      <div class="" slot="actions" slot-scope="props">
        <button
          class="btn-floating waves-effect waves-light tooltipped red"
          data-position="top"
          data-tooltip="Delete drug"
          @click="deleteGiftCard(props.rowData.id, props.rowIndex)"
        >
          <i class="large material-icons">delete</i>
        </button>
      </div>
    </DataTable>
  </div>
</template>

<script>
import { EventBus } from "@/EventBus.js";

import { DataTable } from "v-datatable-light";
import tableStyles from "@/assets/js/TableStyles";
export default {
  mounted() {
    this.getAllItems();
  },
  data() {
    return {
      isLoading: false,
      tableData: [],
      tableStyles: tableStyles,
      searchtext: null,
      headerFields: [
        {
          label: "id",
          name: "id"
        },
        {
          label: "code",
          name: "code"
        },
        {
          label: "value",
          name: "value"
        },
        {
          label: "expiration date",
          name: "expiryDate"
        },
        {
          label: "used",
          name: "used"
        },
        {
          label: "claiming user id",
          name: "claimingUserId"
        },
        {
          label: "actions",
          name: "actions",
          customElement: "actions"
        }
      ]
    };
  },
  components: {
    DataTable,
    Loading: () => import("@/components/Loading")
  },
  methods: {
    search() {
      alert("search");
    },
    async getAllItems() {
      this.isLoading = true;
      const response = await this.axios.get(
        `${this.$store.state.baseApiUrl}GiftCards`
      );
      this.tableData = response.data; //add the listings in array in the table data to be viewed
      this.isLoading = false;
    },
    async getSearchedGiftCards() {
      const response = await this.axios.get(
        `${this.$store.state.baseApiUrl}searchGiftCards/` + this.searchtext
      );
      this.tableData = response.data; //add the users in array in the table data to be viewed
    },
    async deleteGiftCard(giftCardId, giftCardIndex) {
      try {
        //request to delete the drug
        await this.axios.delete(
          `${this.$store.state.baseApiUrl}GiftCards/${giftCardId}`
        );

        //remove the drug from the component
        this.tableData.splice(giftCardIndex, 1);
        //push notification on deleting successfully
        EventBus.$emit("successNotification", "Gift card deleted successfully");
      } catch (err) {
        //push notification with the error
        EventBus.$emit(
          "errorNotification",
          "Error occured, please try again later"
        );
      }
    }
  },
  watch: {
    searchtext(newValue) {
      this.searchtext = newValue;
      if (newValue == "") {
        this.getAllItems();
        console.log("all");
      } else {
        this.getSearchedGiftCards();
        console.log("searched");
      }
    }
  }
};
</script>
