<template>

  <div>
    <SfLoader
    :class="{ 'loading--categories': productLoading }"
    :loading="productLoading">
    <div id="product" >
      <div class="product">
        <LazyHydrate when-idle>
          <SfGallery :images="productGallery" class="product__gallery" />
        </LazyHydrate>

        <div class="product__info">
          <div class="product__header">
            <p class="product__brand">{{ productGetters.getBrand(product) }}</p>
            
            <SfHeading
              :title="productGetters.getName(product)"
              :level="3"
              class="sf-heading--no-underline sf-heading--left"
            />
            <SfIcon
              icon="drag"
              size="xxl"
              color="var(--c-text-disabled)"
              class="product__drag-icon smartphone-only"
            />
          </div>
          <div class="product__price-and-rating">
            <SfPrice
              :regular="$n(productGetters.getPrice(product).regular, 'currency')"
              :special="$n(productGetters.getPrice(product).regular, 'currency') === $n(productGetters.getPrice(product).special, 'currency')? '': $n(productGetters.getPrice(product).special, 'currency')"
            />
            <!-- <div>
              <div class="product__rating">
                <SfRating :score="averageRating" :max="5" @onClick="alert(2)" />
                <a v-if="!!totalReviews" href="#" class="product__count">({{ totalReviews }})</a>
              </div>
              <SfButton class="sf-button--text">{{ $t('Read all reviews') }}</SfButton>
            </div> -->
          </div>
          <div>
            <p
              class="product__description desktop-only"
              v-html="productGetters.getShortDescription(product)"
            ></p>
            <template
              v-for="(option, optionKey) in options"
            >
              <div
                v-if="option.group_type === 'color'"
                class="product__colors desktop-only"
              >
                <p class="product__color-label">{{ $t('Color') }}:</p>
                <SfColor
                  v-for="(color, i) in option.attributes"
                  :key="i"
                  :selected='color.selected'
                  :color="color.html_color_code"
                  class="product__color"
                  @click="updateFilter({ [option.name.replace(/\s+/g, '_')]: `${optionKey}-${i}-${color.name.replace(/[\s&/#,+()$~%.':*?<>{}]/g, '_')}`})"
                />
              </div>

              <SfSelect
                v-else
                :key="optionKey"
                @input="input => updateFilter({ [option.name.replace(/\s+/g, '_')]: input})"
                :label="option.name"
                :value="selectedAttribute(optionKey)"
                :class="`sf-select--underlined product__select-${optionKey.toLowerCase()}`"
              >
                <SfSelectOption
                  v-for="(attribute, attributeKey) in option.attributes"
                  :key="attributeKey"
                  :value="`${optionKey}-${attributeKey}-${attribute.name.replace(/[\s&/#,+()$~%.':*?<>{}]/g, '_')}`"
                >{{attribute.name}}</SfSelectOption>
              </SfSelect>
            </template>

            <SfAddToCart
              v-e2e="'product_add-to-cart'"
              :stock="stock"
              v-model="qty"
              :disabled="loading"
              :canAddToCart="stock > 0"
              class="product__add-to-cart"
              @input="handleSpinUpDown($event)"
            >
              <template #add-to-cart-btn>
                <SfButton
                  :disabled="loading || quantity <= 0 || disableAddToCart"
                  class="sf-add-to-cart__button"
                  v-on="$listeners"
                  @click="addingToCart({ product, quantity: parseInt(qty) } )"
                >
                 Añadir al carrito
                </SfButton>
              </template>
            </SfAddToCart>

            <div v-if="disableAddToCart" class="product-message">
              {{productMessage}}
            </div>

            <span @click="setShowDescription()" class="see-more">Ver más</span>
          </div>

          <LazyHydrate when-idle>
            <SfTabs :open-tab="1" class="product__tabs" v-if="showDescription">
              <SfTab title="Descripción">
                <div class="product__description" v-html="productGetters.getDescription(product)"></div>
                <SfProperty
                  class="product__property"
                  name="Category"
                  :value="productGetters.getCategory(product)"
                ></SfProperty>

                <SfProperty
                  v-for="(property, i) in productGetters.getProductInfo(product)"
                  :key="i"
                  :name="property.name"
                  :value="property.value"
                  class="product__property"
                >
                  <template v-if="property.name === 'Category'" #value>
                    <SfButton class="product__property__button sf-button--text">{{ property.value }}</SfButton>
                  </template>
                </SfProperty>
              </SfTab>
              <!-- <SfTab title="Read reviews">
                <SfButton
                  v-if="isAuthenticated"
                  class="before-results__button"
                  style="margin-bottom:60px"
                  @click="addReviewModal=true"
                >ADD REVIEW</SfButton>
                <p  v-else>You must be logged in to write comment</p>
               
                <SfReview
                  v-for="review in reviews"
                  :key="reviewGetters.getReviewId(review)"
                  :author="reviewGetters.getReviewAuthor(review)"
                  :date="reviewGetters.getReviewDate(review)"
                  :message="reviewGetters.getReviewMessage(review)"
                  :rating="reviewGetters.getReviewRating(review)"
                  read-more-text="Read more"
                  hide-full-text="Read less"
                  class="product__review"
                />
                <LazyHydrate>
                  <SfPagination
                    v-if="Math.ceil(totalReviews/totalReviewPerPage) > 1"
                    class="products__pagination desktop-only"
                    :current="currentPage"
                    :total="Math.ceil(totalReviews/totalReviewPerPage)"
                    :visible="5"
                  >
                    <template #number="{page}">
                    <span
                      class="sf-pagination__item arrow"
                      :class="{'current': currentPage === page}"
                      @click="goNext(page)"
                    >{{page}}</span>
                    </template>

                    <template #next="{isDisabled, go, next}">
                      <span @click="goNext(currentPage + 1)" class="arrow">&#8594</span>
                    </template>

                    <template #prev="{isDisabled, go, prev}">
                      <span @click="goNext(currentPage - 1)" class="arrow">&#8592</span>
                    </template>
                  </SfPagination>
                </LazyHydrate>
              </SfTab> -->
              <SfTab title="Información adicional" class="product__additional-info">
                <div class="product__additional-info">
                  <p class="product__additional-info__title">{{ $t('Brand') }}</p>
                  <p>{{ productGetters.getBrand(product) }}</p>
                </div>
              </SfTab>
            </SfTabs>
          </LazyHydrate>
        </div>
      </div>
    </div>
  </SfLoader>

    <LazyHydrate when-visible>
        <div class="similar-products">
          <SfHeading title="- Podría interesarte - " :level="2"/>
          <nuxt-link :to="localePath('/c/women')" class="smartphone-only">
            {{ $t('See all') }}
          </nuxt-link>
        </div>
      </LazyHydrate>

      <LazyHydrate when-visible>
        <SfCarousel class="carousel" :settings="{ peek: 16, breakpoints: { 1023: { peek: 0, perView: 2 } } }">
          <template #prev="{go}">
            <SfArrow
              aria-label="prev"
              class="sf-arrow--left sf-arrow--long"
              @click="go('prev')"
            />
          </template>
          <template #next="{go}">
            <SfArrow
              aria-label="next"
              class="sf-arrow--right sf-arrow--long"
              @click="go('next')"
            />
          </template>
          <SfCarouselItem class="carousel__item" v-for="(product, i) in relatedProducts" :key="i" style="margin-right: 10px;">
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
  </div>

</template>
<script>
import {
  SfProperty,
  SfHeading,
  SfPrice,
  SfRating,
  SfSelect,
  SfAddToCart,
  SfTabs,
  SfGallery,
  SfIcon,
  SfImage,
  SfBanner,
  SfAlert,
  SfSticky,
  SfReview,
  SfBreadcrumbs,
  SfButton,
  SfColor,
  SfPagination,
  SfLoader,
  SfCarousel,
  SfProductCard
} from '@storefront-ui/vue';

import InstagramFeed from '~/components/InstagramFeed.vue';
import AddReview from '~/components/AddReview.vue';
import RelatedProducts from '~/components/RelatedProducts.vue';
import { ref, computed } from '@nuxtjs/composition-api';
import {
  useProduct,
  useCart,
  productGetters,
  useReview,
  useUser,
  reviewGetters,
  useCheckProduct
} from '@vue-storefront/prestashop';
import { onSSR } from '@vue-storefront/core';
import LazyHydrate from 'vue-lazy-hydration';
import cacheControl from './../helpers/cacheControl';
import useUiNotification from '~/composables/useUiNotification';

export default {
  name: 'Product',
  transition: 'fade',
  middleware: cacheControl({
    'max-age': 60,
    'stale-when-revalidate': 5
  }),
  setup(props, context) {
    const qty = ref(1);
    const { id } = context.root.$route.params;
    const { products, search, loading: productLoading } = useProduct('products');
    const { product: checkedProduct, check: checkProduct } = useCheckProduct();
    const showDescription = ref(false);
    const { addItem: addItemToCart, isInCart } = useCart();
    const {
      products: featureProducts,
      search: searchRelatedProducts,
      loading: relatedLoading
    } = useProduct('relatedProducts');
    const { addItem, loading } = useCart();
    const { reviews: productReviews, search: searchReviews } = useReview(
      'productReviews'
    );
    const { send: sendNotification } = useUiNotification();
    // const pagination = computed(() => facetGetters.getPagination(result.value));
    const { isAuthenticated } = useUser();

    const selectedAttrId = ref(false);
    const selectedGroupId = ref(false);
    const disableAddToCart = ref(false);
    const productMessage = ref(false);

    const product = computed(
      () =>
        productGetters.getFiltered(products.value, {
          master: true,
          attributes: context.root.$route.query
        })[0]
    );
    const options = computed(() =>
      productGetters.getAttributes(product.value)
    );
    const categories = computed(() =>
      productGetters.getCategoryIds(product.value)
    );
    const reviews = computed(() =>
      reviewGetters.getItems(productReviews.value.psdata)
    );
    const quantity = computed(() =>
      productGetters.getQuantity(product.value)
    );

    const setShowDescription = () => {
      console.log("Description");
      if(!showDescription.value){
        showDescription.value = true;
      }else{
        showDescription.value = false;

      }

    };
    const selectedAttribute = (optionKey) => {
      const option = options.value[Number(optionKey)];
      const attrs = option.attributes;
      for (const key in attrs) {
        if (attrs[key].selected) {
          selectedAttrId.value = key;
          selectedGroupId.value = optionKey;
          return `${optionKey}-${key}-${attrs[key].name.replace(/[\s&/#,+()$~%.':*?<>{}]/g, '_')}`;
        }
      }
      return null;
    };

    // TODO: Breadcrumbs are temporary disabled because productGetters return undefined. We have a mocks in data
    // const breadcrumbs = computed(() => productGetters.getBreadcrumbs ? productGetters.getBreadcrumbs(product.value) : props.fallbackBreadcrumbs);
    const productGallery = computed(() =>
      productGetters.getGallery(product.value).map((img) => ({
        mobile: { url: img.small },
        desktop: { url: img.normal },
        big: { url: img.big },
        alt: product.value.name ? product.value.name : 'product alt'
      }))
    );

    onSSR(async () => {
      let variant = context.root.$route.query;
      if (variant && Object.keys(variant).length === 0) {
        variant = null;
      }
      await search({ id, variant: variant });
      await searchRelatedProducts({ featured: true });
      await searchReviews({ productId: id, page: '1' });
    });

    const updateFilter = (filter) => {
      const variant = context.root.$route.query;
      context.root.$router.push({
        path: context.root.$route.path,
        query: {
          ...variant,
          ...filter
        }
      });
    };

    const totalReviews = computed(() =>
      reviewGetters.getTotalReviews(productReviews.value.psdata)
    );

    const totalReviewPerPage = computed(() =>
      reviewGetters.getReviewsPage(productReviews.value.psdata)
    );

    const goNext = (item) => {
      if (item < 1 || Math.ceil(totalReviews / totalReviewPerPage) < item) {
        return false;
      }

      this.currentPage = item;
      onSSR(async () => {
        searchReviews({ productId: this.id, page: this.currentPage });
      });
    };

    return {
      selectedAttribute,
      updateFilter,
      searchReviews,
      sendNotification,
      showDescription,
      setShowDescription,
      product,
      reviews,
      productReviews,
      reviewGetters,
      averageRating: computed(() =>
        productGetters.getAverageRating(product.value)
      ),
      totalReviews,
      totalReviewPerPage,
      relatedProducts: computed(() =>
        productGetters.getFeaturedProductsFiltered(featureProducts.value)
      ),
      relatedLoading,
      options,
      categories,
      qty,
      addItem,
      loading,
      productLoading,
      productGetters,
      productGallery,
      isAuthenticated,
      goNext,
      quantity,
      productMessage,
      disableAddToCart,
      checkedProduct,
      checkProduct,
      selectedAttrId,
      selectedGroupId
    };
  },
  methods: {
    HandleAddToCart(productObj) {
      this.addItemToCart(productObj).then(() => {
        this.sendNotification({
          key: 'added_to_cart',
          message: '¡El producto se agregó a tu carrito!',
          type: 'success',
          title: '¡Producto añadido!',
          icon: 'check'
        });
      });
    },
    async addingToCart(Productdata) {
      await this.addItem(Productdata).then(() => {
        this.sendNotification({
          key: 'product_added',
          message: `${Productdata.product.name} se agregó a tu carrito.`,
          type: 'success',
          title: '¡Producto añadido!',
          icon: 'check'
        });
        this.qty = 1;
      });
    },
    async handleSpinUpDown(event) {
      if (!this.selectedGroupId || !this.selectedAttrId) {
        if (this.product.groups.length === 0) {
          await this.checkProduct(this.product.id, 0, 0, event);
        } else {
          const defaultGroupId = Object.keys(this.product.groups)[0];
          const defaultAttr = this.product.groups[defaultGroupId].default;

          await this.checkProduct(this.product.id, defaultGroupId, defaultAttr, event);
        }
      } else {
        await this.checkProduct(this.product.id, this.selectedGroupId, this.selectedAttrId, event);
      }

      if (this.checkedProduct.availability === 'unavailable') {
        this.disableAddToCart = true;
        this.productMessage = this.checkedProduct.availability_message;
      } else {
        this.disableAddToCart = false;
      }
    }
  },
  components: {
    SfAlert,
    SfColor,
    SfProperty,
    SfHeading,
    SfPagination,
    SfPrice,
    SfRating,
    SfSelect,
    SfAddToCart,
    SfTabs,
    SfGallery,
    SfIcon,
    SfImage,
    SfBanner,
    SfSticky,
    SfReview,
    SfBreadcrumbs,
    SfButton,
    InstagramFeed,
    AddReview,
    RelatedProducts,
    LazyHydrate,
    SfLoader,
    SfCarousel,
    SfProductCard
  },
  data() {
    return {
      currentPage: 1,
      addReviewModal: false,
      breadcrumbs: [
        {
          text: 'Home',
          route: {
            link: '#'
          }
        },
        {
          text: 'Category',
          route: {
            link: '#'
          }
        }
      ],
      stock: 1
    };
  }
};
</script>

<style lang="scss" scoped>
#product {
  box-sizing: border-box;
  @include for-desktop {
    max-width: 1272px;
    margin: 0 auto;
  }
}
.arrow {
  cursor: pointer;
  font-size: 26px;
}
.product {
  @include for-desktop {
    display: flex;
    padding-top: 5%;
  }
  &__pagination {
    display: flex;
    justify-content: flex-start;
    margin: var(--spacer-xl) 0 0 0;
  }
  &__info {
    margin: var(--spacer-sm) auto;
    @include for-desktop {
      max-width: 32.625rem;
      margin: 0 0 0 3.5rem;
    }
  }
  &__header {
    --heading-title-color: var(--c-link);
    --heading-title-font-weight: var(--font-weight--bold);
    --heading-padding: 0;
    margin: 0 var(--spacer-sm);
    display: block;
    justify-content: space-between;
    font-weight: bold;
    @include for-desktop {
      --heading-title-font-weight: var(--font-weight--semibold);
      margin: 0 auto;
    }
  }
  &__brand{
    font-weight: bold;
    margin: 0;
  }
  &__drag-icon {
    animation: moveicon 1s ease-in-out infinite;
  }
  &__price-and-rating {
    margin: 0 var(--spacer-sm) var(--spacer-base);
    align-items: center;
    font-size: 20px;
    @include for-desktop {
      display: flex;
      justify-content: space-between;
      margin: var(--spacer-sm) 0 var(--spacer-lg) 0;
    }
  }

  &__rating {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    margin: var(--spacer-xs) 0 var(--spacer-xs);
  }
  &__count {
    @include font(
      --count-font,
      var(--font-weight--normal),
      var(--font-size--sm),
      1.4,
      var(--font-family--secondary)
    );
    color: var(--c-text);
    text-decoration: none;
    margin: 0 0 0 var(--spacer-xs);
  }
  &__description {
    @include font(
      --product-description-font,
      var(--font-weight--light),
      var(--font-size--base),
      1.6,
      var(--font-family--primary)
    );
  }
  &__select-size {
    margin: 0 var(--spacer-sm);
    @include for-desktop {
      margin: 0;
    }
  }
  &__colors {
    @include font(
      --product-color-font,
      var(--font-weight--normal),
      var(--font-size--lg),
      1.6,
      var(--font-family--secondary)
    );
    display: flex;
    align-items: center;
    margin-top: var(--spacer-xl);
  }
  &__color-label {
    margin: 0 var(--spacer-lg) 0 0;
  }
  &__color {
    margin: 0 var(--spacer-2xs);
  }
  &__add-to-cart {
    margin: var(--spacer-base) var(--spacer-sm) 0;
    @include for-desktop {
      margin-top: var(--spacer-2xl);
    }
  }
  &__guide,
  &__compare,
  &__save {
    display: block;
    margin: var(--spacer-xl) 0 var(--spacer-base) auto;
  }
  &__compare {
    margin-top: 0;
  }
  &__tabs {
    --tabs-title-z-index: 0;
    margin: var(--spacer-lg) auto var(--spacer-2xl);
    --tabs-title-font-size: var(--font-size--lg);
    @include for-desktop {
      margin-top: var(--spacer-2xl);
    }
  }
  &__property {
    margin: var(--spacer-base) 0;
    &__button {
      --button-font-size: var(--font-size--base);
    }
  }
  &__review {
    padding-bottom: 24px;
    border-bottom: var(--c-light) solid 1px;
    margin-bottom: var(--spacer-base);
  }
  &__additional-info {
    color: var(--c-link);
    @include font(
      --additional-info-font,
      var(--font-weight--light),
      var(--font-size--sm),
      1.6,
      var(--font-family--primary)
    );
    &__title {
      font-weight: var(--font-weight--normal);
      font-size: var(--font-size--base);
      margin: 0 0 var(--spacer-sm);
      &:not(:first-child) {
        margin-top: 3.5rem;
      }
    }
    &__paragraph {
      margin: 0;
    }
  }
  &__gallery {
    flex: 1;
  }
}

.sf-heading__title.h3{
  font-weight: bold;
}
.breadcrumbs {
  margin: var(--spacer-base) auto var(--spacer-lg);
}
@keyframes moveicon {
  0% {
    transform: translate3d(0, 0, 0);
  }
  50% {
    transform: translate3d(0, 30%, 0);
  }
  100% {
    transform: translate3d(0, 0, 0);
  }
}

.product-message{
  margin-top: 2rem;
  padding: 10px;
  background-color: #ebcccc;
  color: #a94442;
  border-radius: 2px;
}

.sf-price, .sf-price__regular{
    font-size: 30px;
  }

.sf-select__label{
  font-size: 20px;
  margin-bottom: 10px;
  font-weight: bold;
}

.sf-select__dropdown{
  padding: 1rem;
}

.sf-add-to-cart__button{
  background: #0CAA91;
    border-radius: 25px;
}

.sf-add-to-cart__select-quantity{
  border-radius: 25px;
  width: 35%;
  background: #fff;
  border: 1px solid;
}
.sf-quantity-selector__button{
  width: 35%;
  font-weight: bold;
}

.sf-quantity-selector__input{
  width: 30%;
  
}

.see-more{
    padding: 1rem;
    margin-top: 1rem;
    display: block;
    font-weight: bold;
    cursor: pointer;
    font-size: 20px;
}

.carousel {
  margin: 0 calc(0 - var(--spacer-sm)) 0 0;
  @include for-desktop {
    margin: 0;
  }
  &__item {
    margin: 1.375rem 0 2.5rem 0;
    @include for-desktop {
      margin: var(--spacer-xl) 0 var(--spacer-xl) 0;
    }
    &__product {
      --product-card-add-button-transform: translate3d(0, 30%, 0);
    }
  }
  ::v-deep .sf-arrow--long .sf-arrow--right {
    --arrow-icon-transform: rotate(180deg);
    -webkit-transform-origin: center;
    transform-origin: center;
  }
}

.carousel__item{
  text-align: center;
  justify-content: center;

}


.carousel__item .carousel__item__product{

}
.carousel__item img{
  margin-bottom: 10px;
}

.similar-products{
  margin-top: 5rem;
    padding-top: 5rem;
    border-top: 3rem solid #f1f1f1;
}

</style>
