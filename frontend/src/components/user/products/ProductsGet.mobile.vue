<template>
<div>
  <product-patch v-if="isEditProductVisible" v-bind:id="id"></product-patch>

  <mt-cell v-for="product in products" :key="product.id" :title="product.title">
    <mt-button size="small" @click="showPatchForm(product.id)" type="default">{{ $t('button.first') }}</mt-button>
    <mt-button size="small" @click="deleteProduct(product.id)" type="danger">{{ $t('button.second') }}</mt-button>
  </mt-cell>
</div>
</template>

<script>
import ProductsGetMixin from './ProductsGet.mixin'
import ProductPatch from './product/ProductPatch.mobile'
import {
  Toast
} from 'mint-ui'

export default {
  mixins: [ProductsGetMixin],
  components: {
    ProductPatch
  },

  methods: {
    showPatchForm (id) {
      this.$set(this, 'id', id)
      this.$store.commit('SET_IS_EDIT_PRODUCT_VISIBLE', true)
    },

    async deleteProduct (id) {
      await this.$store.dispatch('deleteProduct', id)
      Toast({
        message: this.$t('product.removed'),
        position: 'bottom',
        duration: 3000
      })
      this.$store.dispatch('getProductsByUser', this.user)
    }
  }
}
</script>
