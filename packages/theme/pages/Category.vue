<template>

  <div>
    <div class="navbar section">
      <div class="navbar__aside desktop-only">
        <LazyHydrate never>
          <SfHeading
            :level="3"
            title="Cateogorías principales"
            class="navbar__title" />
        </LazyHydrate>
      </div>
      
  </div>
  <div id="category">
    <SfBreadcrumbs
      class="breadcrumbs desktop-only"
      :breadcrumbs="breadcrumbs"
    />


    <div class="main section">
      
      <div class="sidebar desktop-only">
        <div class="categories-header">
                    <h2>Categoría</h2>
                    <p>Se han encontrado {{ pagination.totalItems }} productos</p>
                </div>
            <LazyHydrate when-idle>
                <SfLoader
                :class="{ 'loading--categories': loading }"
                :loading="loading">

                <SfAccordion
                    v-e2e="'categories-accordion'"
                    :show-chevron="true"
                >
                    <SfAccordionItem
                    v-for="(cat, i) in categoryTree && categoryTree.items"
                    :key="i"
                    :header="cat.label"
                    >
                    <template>
                        <SfList class="list">
                        <SfListItem class="list__item">
                            <SfMenuItem
                            :count="cat.count || ''"
                            :label="cat.label"
                            >
                            <!-- <template #label>
                                <nuxt-link
                                :to="localePath(th.getCatLink(cat))"
                                :class="cat.isCurrent ? 'sidebar--cat-selected' : ''"
                                >
                                Todos
                                </nuxt-link>
                            </template> -->
                            </SfMenuItem>
                        </SfListItem>
                        <SfListItem
                            class="list__item"
                            v-for="(subCat, j) in cat.items"
                            :key="j"
                        >
                            <SfMenuItem
                            :count="subCat.count || ''"
                            :label="subCat.label"
                            >
                            <template #label="{ label }">
                                <nuxt-link
                                :to="localePath(th.getCatLink(subCat))"
                                :class="subCat.isCurrent ? 'sidebar--cat-selected' : ''"
                                >
                                {{ label }}
                                </nuxt-link>
                            </template>
                            </SfMenuItem>
                        </SfListItem>
                        </SfList>
                    </template>
                    </SfAccordionItem>
                </SfAccordion>
                </SfLoader>
            </LazyHydrate>
      </div>
      <SfLoader :class="{ loading }" :loading="loading">
        <div class="products" v-if="!loading">
          <CategoryPageHeader :pagination="pagination"/>
          <transition-group
            v-if="isCategoryGridView"
            appear
            name="products__slide"
            tag="div"
            class="products__grid"
          >
          

            <SfProductCard
              v-e2e="'category-product-card'"
              v-for="(product, i) in products"
              :key="productGetters.getSlug(product)"
              :style="{ '--index': i }"
              :title="productGetters.getName(product)"
              :image="product.coverImageLarge"
              :regular-price="$n(productGetters.getPrice(product).regular, 'currency')"
              :special-price="$n(productGetters.getPrice(product).regular, 'currency') === $n(productGetters.getPrice(product).special, 'currency')? '': $n(productGetters.getPrice(product).special, 'currency')"
              :max-rating="5"
              :score-rating="productGetters.getAverageRating(product)"
              :show-add-to-cart-button="true"
              :is-in-wishlist="isInWishlist({ product })"
              :is-added-to-cart="isInCart({ product })"
              :link="localePath(`/p/${productGetters.getId(product)}/${product.slug}`)"
              class="products__product-card"
              @click:wishlist="!isInWishlist({ product }) ? addItemToWishlist({ product }) : removeProductFromWishlist(product)"
              @click:add-to-cart="HandleAddToCart({ product, quantity: 1 })"
            />
          </transition-group>
          <transition-group
            v-else
            appear
            name="products__slide"
            tag="div"
            class="products__list"
          >
            <SfProductCardHorizontal
              v-e2e="'category-product-card'"
              v-for="(product, i) in products"
              class="products__product-card-horizontal"
              :key="productGetters.getSlug(product)"
              :style="{ '--index': i }"
              :title="productGetters.getName(product)"
              :description="productGetters.getDescription(product)"
              :image="product.coverImageLarge"
              :regular-price="$n(productGetters.getPrice(product).regular, 'currency')"
              :special-price="productGetters.getPrice(product).special && $n(productGetters.getPrice(product).special, 'currency')"
              :max-rating="5"
              :score-rating="3"
              :qty="1"
              :is-in-wishlist="isInWishlist({ product })"
              :link="localePath(`/p/${productGetters.getId(product)}/${product.slug}`)"
              @input="productsQuantity[product._id] = $event"
              @click:wishlist="!isInWishlist({ product }) ? addItemToWishlist({ product }) : removeProductFromWishlist(product)"
              @click:add-to-cart="HandleAddToCart({ product, quantity: 1})"
            >

              <template #configuration>
                <SfProperty
                  class="desktop-only"
                  name="Size"
                  value="XS"
                  style="margin: 0 0 1rem 0;"
                />
                <SfProperty class="desktop-only" name="Color" value="white" />
              </template>
              <template #actions>
                <SfButton
                  class="sf-button--text desktop-only"
                  style="margin: 0 0 1rem auto; display: block;"
                  @click="() => {}"
                >
                  {{ $t('Save for later') }}
                </SfButton>
              </template>
            </SfProductCardHorizontal>
          </transition-group>

          <LazyHydrate on-interaction>
            <SfPagination
              v-if="!loading"
              class="products__pagination desktop-only"
              v-show="pagination.totalPages > 1"
              :current="pagination.currentPage"
              :total="pagination.totalPages"
              :visible="5"
            />
          </LazyHydrate>

          <div
            v-show="pagination.totalPages > 1"
            class="products__show-on-page"
          >
            <span class="products__show-on-page__label">{{ $t('Show on page') }}</span>
            <LazyHydrate on-interaction>
              <SfSelect
                :value="pagination.itemsPerPage.toString()"
                class="products__items-per-page"
                @input="th.changeItemsPerPage"
              >
                <SfSelectOption
                  v-for="option in pagination.pageOptions"
                  :key="option"
                  :value="option"
                  class="products__items-per-page__option"
                >
                  {{ option }}
                </SfSelectOption>
              </SfSelect>
            </LazyHydrate>
          </div>
        </div>
      </SfLoader>
    </div>
    <LazyHydrate when-visible>
        <div class="similar-products">
          <SfHeading title="- Marcas exclusivas - " :level="2"/>
          <nuxt-link :to="localePath('/c/women')" class="smartphone-only">
            {{ $t('See all') }}
          </nuxt-link>
        </div>
      </LazyHydrate>

      <LazyHydrate when-visible>
        <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/homepage/marcas.png" srcset="" sizes=""  class="sf-image-loaded" style="height: auto !important; width: 100% !important">
              </nuxt-link>
      </LazyHydrate>
  </div>

  </div>


</template>

<script>
import {
  SfSidebar,
  SfButton,
  SfList,
  SfIcon,
  SfHeading,
  SfMenuItem,
  SfFilter,
  SfProductCard,
  SfProductCardHorizontal,
  SfPagination,
  SfAccordion,
  SfSelect,
  SfBreadcrumbs,
  SfLoader,
  SfColor,
  SfProperty
} from '@storefront-ui/vue';
import { computed, ref, onMounted } from '@nuxtjs/composition-api';
import { useCart, useWishlist, productGetters, useFacet, facetGetters, wishlistGetters } from '@vue-storefront/prestashop';
import { useUiHelpers, useUiState, useUiNotification } from '~/composables';
import { onSSR } from '@vue-storefront/core';
import LazyHydrate from 'vue-lazy-hydration';
import Vue from 'vue';
import cacheControl from './../helpers/cacheControl';
import CategoryPageHeader from '~/components/CategoryPageHeader';
import { addBasePath } from '@vue-storefront/core';

// TODO(addToCart qty, horizontal): https://github.com/vuestorefront/storefront-ui/issues/1606
export default {
  transition: 'fade',
  middleware: cacheControl({
    'max-age': 60,
    'stale-when-revalidate': 5
  }),
  setup(props, context) {
    const th = useUiHelpers();
    const uiState = useUiState();
    const { addItem: addItemToCart, isInCart } = useCart();
    const { result, search, loading } = useFacet();
    const { send: sendNotification } = useUiNotification();
    const { addItem: addItemToWishlist, isInWishlist, removeItem: removeItemFromWishlist, wishlist } = useWishlist();

    const productsQuantity = ref({});
    const products = computed(() => facetGetters.getProducts(result.value));
    console.log(products);
    const categoryTree = computed(() => facetGetters.getCategoryTreeGeneral(result.value));

    const breadcrumbs = computed(() => facetGetters.getBreadcrumbs(result.value));
    const sortBy = computed(() => facetGetters.getSortOptions(result.value));
    const facets = computed(() => facetGetters.getGrouped(result.value));
    const pagination = computed(() => facetGetters.getPagination(result.value));

    const selectedFilters = ref({});
    const setSelectedFilters = () => {
      if (!facets.value.length || Object.keys(selectedFilters.value).length) return;
      selectedFilters.value = facets.value.reduce((prev, curr) => ({
        ...prev,
        [curr.id]: curr.options
          .filter(o => o.selected)
          .map(o => o.id)
      }), {});
    };

    onSSR(async () => {
      await search(th.getFacetsFromURL());
      setSelectedFilters();
    });

    const { changeFilters, isFacetColor } = useUiHelpers();
    const { toggleFilterSidebar } = useUiState();

    onMounted(() => {
      context.root.$scrollTo(context.root.$el, 2000);
      setSelectedFilters();
    });

    const isFilterSelected = (facet, option) => (selectedFilters.value[facet.id] || []).includes(option.id);

    const selectFilter = (facet, option) => {
      if (!selectedFilters.value[facet.id]) {
        Vue.set(selectedFilters.value, facet.id, []);
      }

      if (selectedFilters.value[facet.id].find(f => f === option.id)) {
        selectedFilters.value[facet.id] = selectedFilters.value[facet.id].filter(f => f !== option.id);
        changeFilters(selectedFilters.value);
        return;
      }

      selectedFilters.value[facet.id].push(option.id);
      changeFilters(selectedFilters.value);
    };

    const clearFilters = () => {
      toggleFilterSidebar();
      selectedFilters.value = {};
      changeFilters(selectedFilters.value);
    };

    const applyFilters = () => {
      toggleFilterSidebar();
    };

    const removeProductFromWishlist = (productItem) => {
      const productsInWhishlist = computed(() => wishlistGetters.getItems(wishlist.value));
      const product = productsInWhishlist.value.find(wishlistProduct => wishlistProduct.variant.sku === productItem.sku);
      removeItemFromWishlist({ product });
    };

    return {
      ...uiState,
      sendNotification,
      th,
      products,
      categoryTree,
      loading,
      productGetters,
      pagination,
      sortBy,
      facets,
      breadcrumbs,
      addItemToWishlist,
      removeProductFromWishlist,
      isInWishlist,
      addItemToCart,
      isInCart,
      productsQuantity,
      isFacetColor,
      selectFilter,
      isFilterSelected,
      selectedFilters,
      clearFilters,
      applyFilters,
      addBasePath
    };
  },
  methods: {
    HandleAddToCart(productObj) {
      this.addItemToCart(productObj).then(() => {
        this.sendNotification({
          key: 'added_to_cart',
          message: '¡Se agregó a tu carrito!',
          type: 'success',
          title: '¡Producto agregado!',
          icon: 'check'
        });
      });
    }
  },
  components: {
    CategoryPageHeader,
    SfButton,
    SfSidebar,
    SfIcon,
    SfList,
    SfFilter,
    SfProductCard,
    SfProductCardHorizontal,
    SfPagination,
    SfMenuItem,
    SfAccordion,
    SfSelect,
    SfBreadcrumbs,
    SfLoader,
    SfColor,
    SfHeading,
    SfProperty,
    LazyHydrate
  }
};
</script>

<style lang="scss" scoped>
#category {
  box-sizing: border-box;
  @include for-desktop {
    max-width: 95%;
    margin: 0 auto;
  }
}
.main {
  &.section {
    padding: var(--spacer-xs);
    @include for-desktop {
      padding: 0;
    }
  }
}
.breadcrumbs {
  margin: var(--spacer-base) auto var(--spacer-lg);
}
.navbar {
  position: relative;
  display: flex;
  border: 1px solid var(--c-light);
  border-width: 0 0 1px 0;
  @include for-desktop {
    border-width: 1px 0 1px 0;
  }
  &.section {
    padding: var(--spacer-sm);
    @include for-desktop {
      padding: 0;
      background: #006E6C;
      margin-bottom: 2rem;
    }
  }
  &__aside {
    align-items: center;
    padding: var(--spacer-sm) 0;
    flex: 0 0 15%;
    padding: 5px;
    max-width: 95%;
    display: block;
    width: 100%;
    margin: 0 auto;
  }
  &__title {
    --heading-title-font-weight: var(--font-weight--semibold);
    --heading-title-font-size: var(--font-size--xl);
  }
}
.main {
  display: flex;
}
.list {
  --menu-item-font-size: var(--font-size--sm);
  &__item {
    &:not(:last-of-type) {
      --list-item-margin: 0 0 var(--spacer-sm) 0;
    }
    .nuxt-link-exact-active {
      text-decoration: underline;
    }
  }
}
.products {
  box-sizing: border-box;
  flex: 1;
  margin: 0;
  &__grid {
    justify-content: center;
    @include for-desktop {
      justify-content: flex-start;
    }
  }
  &__grid,
  &__list {
    display: flex;
    flex-wrap: wrap;
  }
  &__product-card {
    --product-card-title-margin: var(--spacer-base) 0 0 0;
    --product-card-title-font-weight: var(--font-weight--medium);
    --product-card-title-margin: var(--spacer-xs) 0 0 0;
    flex: 1 1 50%;
    @include for-desktop {
      --product-card-title-font-weight: var(--font-weight--normal);
      --product-card-add-button-bottom: var(--spacer-base);
      --product-card-title-margin: var(--spacer-sm) 0 0 0;
    }
  }
  &__product-card-horizontal {
    flex: 0 0 100%;
    @include for-mobile {
      ::v-deep .sf-image {
        --image-width: 5.3125rem;
        --image-height: 7.0625rem;
      }
    }
  }
  &__slide-enter {
    opacity: 0;
    transform: scale(0.5);
  }
  &__slide-enter-active {
    transition: all 0.2s ease;
    transition-delay: calc(0.1s * var(--index));
  }
  @include for-desktop {
    &__grid {
      margin: var(--spacer-sm) 0 0 var(--spacer-sm);
    }
    &__pagination {
      display: flex;
      justify-content: flex-start;
      margin: var(--spacer-xl) 0 0 0;
    }
    &__product-card-horizontal {
      margin: var(--spacer-lg) 0;
    }
    &__product-card {
      flex: 1 1 25%;
    }
    &__list {
      margin: 0 0 0 var(--spacer-sm);
    }
  }
  &__show-on-page {
    display: flex;
    justify-content: flex-end;
    align-items: baseline;
    &__label {
      font-family: var(--font-family--secondary);
      font-size: var(--font-size--sm);
    }
  }
}
.sidebar {
  flex: 0 0 20%;
  padding: var(--spacer-sm);
  border: 1px solid var(--c-light);
  border-width: 0 1px 0 0;
}
.loading {
  margin: var(--spacer-3xl) auto;
  @include for-desktop {
    margin-top: 6.25rem;
  }
  &--categories {
    @include for-desktop {
      margin-top: 3.75rem;
    }
  }
}

.navbar.section{
  margin-top: 2rem;
}

.navbar.section h3{
  color: #fff;
  font-size: 16px;
}


#category .sf-accordion .sf-accordion-item button{
  padding: 1rem 0.5rem;
}

.similar-products {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: var(--spacer-2xs);
  padding-top: 3rem;
  --heading-padding: 0;
  border-bottom: 1px var(--c-light) solid;
  @include for-desktop {
    border-bottom: 0;
    justify-content: center;
    padding-bottom: 0;
  }
}

#category .sidebar{
    padding: 0px;
    border-radius: 5px;
    border: 1px solid #9E9E9E;
  }

#category .categories-header{
    padding: 1rem;
    text-align: center;
  }
  
  
  #category .sf-accordion .sf-accordion-item{
    border-bottom: none;
    padding: 5px 20px;
  }
  
  #category .sf-accordion .sf-accordion-item button{
    padding: 2px 1rem;
  }
</style>
