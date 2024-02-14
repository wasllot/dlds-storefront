<template>

  <div>
    <div id="cart">
      <div class="cart-content">

        <span class="cart-content-title"><img src="/icons/cart.svg"/>Tu carro de compras ({{ totalItems }} productos)</span>
        

        <transition name="sf-fade" mode="out-in">
          <div v-if="totalItems" key="my-cart" class="my-cart">
            <div class="collected-product-list">
              <transition-group name="sf-fade" tag="div">
                <SfCollectedProduct
                  v-for="product in products"
                  v-e2e="'collected-product'"
                  :key="cartGetters.getItemSku(product)"
                  :image="addBasePath(cartGetters.getItemImage(product))"
                  :stock="99999"
                  class="collected-product"
                >
                  <!-- @TODO: remove if https://github.com/vuestorefront/storefront-ui/issues/2022 is done -->
                  <template #more-actions>
                    <div class="product-cart-details">

                      <div class="product-cart-column">
                        <h2 class="product-cart-title">{{ cartGetters.getItemName(product)  }}</h2>

                        <div class="sf-collected-product__quantity-wrapper">
                          <SfQuantitySelector
                            :disabled="loading"
                            :qty="cartGetters.getItemQty(product)"
                            class="sf-collected-product__quantity-selector"
                            @input="updateItemQty({ product, quantity: $event })"
                          />
                        </div>
                      </div>
                      <div class="product-cart-column product-info">
                          <span class="regular-price">{{ $n(cartGetters.getItemPrice(product).regular, 'currency')  }}</span>
                          <button class="remove-product" @click:remove="removeItem({ product })"><img src="/icons/trash.svg"/>Eliminar</button>

                      </div>
                    </div>

                  </template>
                </SfCollectedProduct>
              </transition-group>
            </div>
          </div>
          <div v-else key="empty-cart" class="empty-cart">
            <div class="empty-cart__banner">
              <SfImage
                :alt="$t('Empty bag')"
                class="empty-cart__image"
                :src="addBasePath('/icons/empty-cart.svg')"
              />
              <SfHeading
                title="Tu carrito está vacío"
                :level="2"
                class="empty-cart__heading"
                :description="$t('Empty')"
              />
            </div>
          </div>
        </transition>

      </div>
      <div class="cart-resume">
        
          <div class="medios-de-pago">
            <span>Medios de pago <img src="/icons/medios-de-pago.png" alt=""></span>
          </div>
          <div class="coupon-code">
            <span><img src="/icons/cupon.svg" alt=""/>Ingresa tu código promocional</span>
            <div class="coupon-code-input">
              <form action="">
                <input type="text" name="" id="">
                <input type="submit" value="APLICAR">
              </form>
            </div>
          </div>
          <div v-if="totalItems">
            <div v-if="errors.length > 0" class="cart-error">
              {{errors[0]}}
            </div>

            <div class="cart-resume-side">

                <h3 class="cart-resume-title">Resumen de compra</h3>
                <div class="cart-resume-tr">
                  <h4>Costo de tus productos </h4> 
                  <span>{{ $n(totals.subtotal, 'currency')  }}</span>
                </div>
                <div class="cart-resume-tr">
                  <h4>Descuentos </h4> 
                  <span>$ 0</span>
                </div>
                

                <hr class="cart-resume-hr">
              <SfProperty
                name="Subtotal"
                class="sf-property--full-width sf-property--large my-cart__total-price"
              >
                <template #value>
                  <SfPrice
                    :regular="$n(totals.subtotal, 'currency')"
                    :special="(totals.special !== totals.subtotal) ? $n(totals.special, 'currency') : 0"
                  />
                  
                </template>
                
              </SfProperty>
              <SfProperty
                name="Total"
                class="sf-property--full-width sf-property--large my-cart__total-price"
              >
                <template #value>
                  <SfPrice
                    :regular="$n(totals.subtotal, 'currency')"
                    :special="(totals.special !== totals.subtotal) ? $n(totals.special, 'currency') : 0"
                  />
                  
                </template>
                
              </SfProperty>

              <nuxt-link :to="isAuthenticated ? localePath({ name: 'shipping' }) : localePath({ name: 'user-account' })" :class="{ disabled: errors.length > 0 }">
                <SfButton
                  class="sf-button--full-width color-secondary"
                  :disabled="errors.length > 0"
                >
                  Ir a pagar
                </SfButton>
              </nuxt-link>
            </div>


          </div>
          <div v-else>
            <SfButton
              class="sf-button--full-width color-primary"
              @click="toggleCartSidebar"
            >Seguir comprando</SfButton
            >
          </div>
      </div>



    </div>

    <hr class="cart-separation">

    <div class="similar-products">
      <SfHeading title="- Podría interesarte - " :level="2"/>
      <nuxt-link :to="localePath('/c/women')" class="smartphone-only">
        {{ $t('See all') }}
      </nuxt-link>
    </div>

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
  </div>


</template>
<script>
import {
  SfSidebar,
  SfHeading,
  SfCarousel,
  SfProductCard,
  SfButton,
  SfIcon,
  SfProperty,
  SfPrice,
  SfCollectedProduct,
  SfImage,
  SfQuantitySelector,
  SfArrow
} from '@storefront-ui/vue';
import { computed } from '@nuxtjs/composition-api';
import { useCart, useUser, cartGetters, useProduct, productGetters, useCheckProduct} from '@vue-storefront/prestashop';
import { useUiState } from '~/composables';
import { onSSR } from '@vue-storefront/core';
import { addBasePath } from '@vue-storefront/core';
import { useUiNotification } from '~/composables';
import RelatedProducts from '~/components/RelatedProducts.vue';

export default {
  name: 'CartPage',
  components: {
    SfSidebar,
    SfButton,
    SfCarousel,
    SfProductCard,
    SfHeading,
    SfIcon,
    SfProperty,
    SfPrice,
    SfCollectedProduct,
    SfImage,
    SfQuantitySelector,
    RelatedProducts,
    SfCarousel,
    SfProductCard,
    SfArrow
  },
  setup() {
    const { isCartSidebarOpen, toggleCartSidebar } = useUiState();
    const { cart, removeItem, updateItemQty, load: loadCart, loading } = useCart();
    const { isAuthenticated } = useUser();
    const products = computed(() => cartGetters.getItems(cart.value));
    const { product: checkedProduct, check: checkProduct } = useCheckProduct();
    const totals = computed(() => cartGetters.getTotals(cart.value));
    const totalItems = computed(() => cartGetters.getTotalItems(cart.value));
    const errors = computed(() => cartGetters.getErrors(cart.value));
    const { addItem: addItemToCart, isInCart } = useCart();
    const { send: sendNotification } = useUiNotification();

    const {
      products: featureProducts,
      search: searchRelatedProducts,
      loading: relatedLoading
    } = useProduct('relatedProducts');

    onSSR(async () => {
      await loadCart();
      await searchRelatedProducts({ featured: true });
    });

    return {
      addBasePath,
      loading,
      isAuthenticated,
      products,
      removeItem,
      updateItemQty,
      sendNotification,
      isCartSidebarOpen,
      toggleCartSidebar,
      totals,
      totalItems,
      cartGetters,
      errors,
      relatedProducts: computed(() =>
        productGetters.getFeaturedProductsFiltered(featureProducts.value)
      ),
      productGetters,
      useCheckProduct,
      relatedLoading
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
  }

};
</script>

<style lang="scss" scoped>
#cart {
  box-sizing: border-box;
  display: flex;
  @include for-desktop {
    max-width: 1240px;
    margin: 0 auto;
  }

  .cart-content{
    flex: 0 0 65%;
    padding: 3rem 0;

    .cart-content-title{
      font-size: 20px;
      margin-bottom: 1rem;
      display: block;

      img{
        width: 25px;
        margin-right: 5px;
      }
    }
  }

  .cart-resume{
    flex: 0 0 35%;
    padding: 2rem 0;

    .medios-de-pago span{
      color: #0caa91;
      font-weight: bold;
      display: flex;
      align-items: center;

      img{
        height: 30px;
      }
    }

    .coupon-code{
      border: 1px solid #0caa916e;
      padding: 1rem;
      margin-top: 2rem;
      margin-bottom: 1rem;
      span{
        font-weight: bold;
        display: flex;
        align-items: center;

        img{
          height: 30px;
          margin-right: 2px;
        }
      }

      &-input{
        margin-top: 1rem;
      }

      &-input input[type=text]{

        padding: 1rem 0.5rem;
        border: 1px solid #0caa91;
        border-radius: 15px;
        margin-right: 5px;
        width: 60%;

      }

      &-input input[type=submit]{
        background-color: #0caa91;
        color: #fff;
        border: unset;
        padding: 0.7rem;
        border-radius: 5px;

      }

    }

    .cart-resume-side{
        border: 1px solid #0caa916e;
        padding: 1rem;

          h3{

            font-size: 25px;
            margin-bottom: 10px;
            
          }

          .cart-resume-tr{
            display: flex;
            justify-content: space-between;
            align-items: center;

            h4{
              font-weight: 400;
              font-size: 16px;
            }
          }

          .cart-resume-tr:nth-child(1){

            border-bottom: 1px solid #009688;
          }

          .cart-resume-hr{
            margin-top: 1rem;
            margin-bottom: 1rem;
            border-top:  1px solid #009688;
          }

          

        }

        .my-cart__total-price{
          margin: 0;

          span{
            color: #009688;
            font-weight: bolder;
          }

        }


        .color-secondary{
          background-color: #ED1E79;
          width: 65%;
          margin: 10px auto;
        }

  }


}
.cart-summary {
  margin-top: var(--spacer-xl);
}
.my-cart {
  flex: 1;
  display: flex;
  flex-direction: column;
  &__total-items {
    margin: 0;
  }
  &__total-price {
    --price-font-size: var(--font-size--xl);
    --price-font-weight: var(--font-weight--medium);
    margin: 0 0 var(--spacer-base) 0;
  }
}
.empty-cart {
  --heading-description-margin: 0 0 var(--spacer-xl) 0;
  --heading-title-margin: 0 0 var(--spacer-xl) 0;
  --heading-title-color: var(--c-primary);
  --heading-title-font-weight: var(--font-weight--semibold);
  display: flex;
  flex: 1;
  align-items: center;
  flex-direction: column;
  &__banner {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    flex: 1;
  }
  &__heading {
    padding: 0 var(--spacer-base);
  }
  &__image {
    --image-width: 16rem;
    margin: 0 0 var(--spacer-2xl) 7.5rem;
  }
  @include for-desktop {
    --heading-title-font-size: var(--font-size--xl);
    --heading-title-margin: 0 0 var(--spacer-sm) 0;
  }
}
.collected-product-list {
  flex: 1;
}
.collected-product {
  margin: 0 0 var(--spacer-sm) 0;
  max-width: 95%;
  justify-content: space-around;
  height: 200px;
  padding: 2rem 0rem;
  border-top: 1px solid #2F9393;
  border-bottom: 1px solid #2F9393;
  margin-bottom: 0;
  &__properties {
    margin: var(--spacer-xs) 0 0 0;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    align-items: flex-start;
    flex: 2;
    &:first-child {
      margin-bottom: 8px;
    }
  }
  &__actions {
    transition: opacity 150ms ease-in-out;
  }
  &__save,
  &__compare {
    --button-padding: 0;
    &:focus {
      --cp-save-opacity: 1;
      --cp-compare-opacity: 1;
    }
  }
  &__save {
    opacity: var(--cp-save-opacity, 0);
  }
  &__compare {
    opacity: var(--cp-compare-opacity, 0);
  }
  &:hover {
    --cp-save-opacity: 1;
    --cp-compare-opacity: 1;
    @include for-desktop {
      .collected-product__properties {
        display: none;
      }
    }
  }
}

.collected-product:first-child{
  margin-bottom: 0rem;
  border-bottom: unset;
  margin-top: 2rem;
}

.cart-error{
  margin-bottom: 2rem;
  padding: 10px;
  background-color: #ebcccc;
  color: #a94442;
  border-radius: 2px;
}

.disabled {
  pointer-events: none
}

.color-secondary{
  background-color: #0caa91;
}

.sf-collected-product__aside{
  flex: 0 0 30%;
}

.product-cart-details{
  display: flex;
}

.product-cart-details .product-cart-column{
  flex: 0 0 50%;
  position: relative;
  padding: 1rem;
}

.product-cart-details .product-cart-column.product-info{
  display: flex;
  flex-direction: column;
  flex: 100%;
  align-items: center;
  justify-content: space-between;
}

.product-cart-details .regular-price{
  font-size: 24px;
    font-weight: bold;
    color: #0caa91;
}

.product-cart-details .remove-product{
    background: #fff;
    border: none;
    text-align: center;
    font-size: 17px;
    font-weight: bold;
    display: flex;
    align-items: end;
    cursor: pointer;
}

.product-cart-details .remove-product img{
  width: 25px;
  padding-right: 2px;
}

.product-cart-details .product-cart-column .product-cart-title{
  font-size: 18px;
  line-height: 20px;
  color: #585858;
}

.cart-content .sf-collected-product__quantity-wrapper{
  display: none;
}


.product-cart-details .sf-collected-product__quantity-wrapper{
  display: block;
}

img{
    outline-color: unset !important;
    outline-width: unset !important;
    outline-style: unset !important;
}

#layout{
  border-bottom: 20px solid #eee;
}

.cart-separation{
  background: #eee;
  border-top: 20px solid #eee;
}
</style>
