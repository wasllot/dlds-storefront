<template>
  <SfSection :title-heading="title" class="section">
    <LazyHydrate when-visible>
        <SfCarousel class="carousel carousel-counter" :settings="{ peek: 0,  perView: 1, breakpoints: { 1023: { peek: 0, perView: 1 } } }">
          <SfCarouselItem class="carousel__item" v-for="(product, i) in products" :key="i">
            <SfProductCard
              :title="productGetters.getName(product)"
              :image="productGetters.getCoverImage(product)"
              :regular-price="$n(productGetters.getPrice(product).regular, 'currency')"
              :special-price="$n(productGetters.getPrice(product).regular, 'currency') === $n(productGetters.getPrice(product).special, 'currency')? '': $n(productGetters.getPrice(product).special, 'currency')"
              :show-add-to-cart-button="true"
              :link="localePath(`/p/${productGetters.getId(product)}/${productGetters.getSlug(product)}`)"
              class="carousel__item__product"
              @click:add-to-cart="HandleAddToCart({ product, quantity:1 })"
            />
          </SfCarouselItem>
        </SfCarousel>
      </LazyHydrate>
  </SfSection>
</template>

<script lang="ts">

import {
  SfCarousel,
  SfProductCard,
  SfSection,
  SfLoader
} from '@storefront-ui/vue';

import { productGetters } from '@vue-storefront/prestashop';
import LazyHydrate from 'vue-lazy-hydration';

export default {
  name: 'OffersCounter',
  setup() {
    return { productGetters };
  },
  components: {
    SfCarousel,
    SfProductCard,
    SfSection,
    SfLoader,
    LazyHydrate
  },
  props: {
    title: String,
    products: Array,
    loading: Boolean
  }
};
</script>

<style lang="scss" scoped>
.section {
  margin-top: var(--spacer-base);
}

.carousel {
  margin: 0 calc(0 - var(--spacer-sm)) 0 0;
  @include for-desktop {
    margin: 0;
  }
  &__item {
    margin: 1.9375rem 0 2.4375rem 0;
  }
}

.carousel-counter{
  margin: 2rem 0rem;
}

.carousel-counter .sf-carousel__controls{
  display: none;
}

</style>
