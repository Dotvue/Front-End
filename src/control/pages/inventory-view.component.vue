<script>
import InventoryManagementComponent from "../components/inventory-management.component.vue";
import InventoryItemPopUpComponent from "../components/inventory-item-pop-up.component.vue";

export default {
  name: "inventory-view",

  components: {
    InventoryManagementComponent,
    InventoryItemPopUpComponent
  },

  props: {
    inventory: {
      type: Array,
      required: true
    }
  },

  data() {
    return {
      localInventory: [],

      selectedProduct: null,
      isPopUpVisible: false
    }
  },

  methods: {
    _emitProfileLoaded(profileSaved) {
      this.$emit('profile-loaded', profileSaved);
    },
    
    _handleItemSelected(item) {
      this.selectedProduct = item;
      //console.log(this.selectedProduct);
    },

    _handlePopUpState(state) {
      this.isPopUpVisible = state;
      //console.log(this.isPopUpVisible);
    }
  },

  created() {
    this.localInventory = [...this.inventory];
    const profileSaved = JSON.parse(localStorage.getItem("profile"));

    this.localInventory = profileSaved._inventory || [];
    this._emitProfileLoaded(profileSaved);
  }
}
</script>

<template>
  <div class="container">
    <div class="content">
      <div class="inventory">
        <h1 class="inventory__title">Inventory</h1>
        <p class="inventory__info">Track the fluctuation of your products</p>
        <inventory-management-component :inventory="localInventory" @item-selected="_handleItemSelected" @pop-up-state="_handlePopUpState"/>
      </div>

      <inventory-item-pop-up-component v-if="selectedProduct !== null" :product="selectedProduct" :visible="isPopUpVisible" @pop-up-state="_handlePopUpState"/>
    </div>
  </div>
</template>

<style scoped>
.inventory__info {
  margin-top: 10px;
  margin-bottom: 25px;
}
</style>