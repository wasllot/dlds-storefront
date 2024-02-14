<template>
  <div>
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
        <div id="cart">
          <div class="cart-content">

              <span class="cart-content-title"><img src="/icons/cart.svg"/>Mi carro ({{ totalItems }} productos)</span>
              

              <transition name="sf-fade" mode="out-in">
                <div v-if="totalItems" key="my-cart" class="my-cart">
                  <div class="collected-product-list">
                    <transition-group name="sf-fade" tag="div">

                      <div v-for="product in products" class="product-cart-resume">
                        <img :src="addBasePath(cartGetters.getItemImage(product))" alt="">
                        <h3>{{ cartGetters.getItemName(product)  }}</h3>
                        <span>Precio: {{ $n(cartGetters.getItemPrice(product).regular, 'currency')  }}</span>
                        <span>Cantidad: {{ cartGetters.getItemQty(product)  }}</span>
                      </div>
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

                            </div>
                            <div class="product-cart-column product-info">
                                <span class="regular-price">Precio: {{ $n(cartGetters.getItemPrice(product).regular, 'currency')  }}</span>
                            </div>
                            <div class="product-cart-column product-info">
                                <span class="regular-price">Cantidad: {{ cartGetters.getItemQty(product)  }}</span>
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
                <span>{{ $n(discountsAmount, 'currency')  }}</span>
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
                  :regular="$n(totals.total, 'currency')"
                  :special="(totals.special !== totals.total) ? $n(totals.special, 'currency') : 0"
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
</template>
<script>

import { SfHeading, SfProperty, SfCharacteristic, SfPrice, SfCollectedProduct } from '@storefront-ui/vue';
import { computed, ref, defineComponent } from '@nuxtjs/composition-api';
import { useCart, cartGetters } from '@vue-storefront/prestashop';
import { addBasePath } from '@vue-storefront/core';

const CHARACTERISTICS = [
  {
    title: 'Safety',
    description: 'It carefully packaged with a personal touch',
    icon: 'safety'
  },
  {
    title: 'Easy shipping',
    description: 'You’ll receive dispatch confirmation and an arrival date',
    icon: 'shipping'
  },
  {
    title: 'Changed your mind?',
    description: 'Rest assured, we offer free returns within 30 days',
    icon: 'return'
  }
];

export default defineComponent({
  name: 'CartPreview',
  components: {
    SfHeading,
    SfProperty,
    SfCharacteristic,
    SfPrice,
    SfCollectedProduct
  },
  setup() {
    const { cart, removeItem, updateItemQty } = useCart();

    const listIsHidden = ref(false);

    const products = computed(() => cartGetters.getItems(cart.value));
    const totalItems = computed(() => cartGetters.getTotalItems(cart.value));
    const totals = computed(() => cartGetters.getTotals(cart.value));
    const shippingPrice = computed(() => cartGetters.getShippingPrice(cart.value));
    const discounts = computed(() => cartGetters.getDiscounts(cart.value));
    const hasDiscounts = computed(() => discounts.value.length > 0);
    const discountsAmount = computed(
      () => -1 * discounts.value.reduce((a, el) => el.value + a, 0)
    );

    const errors = computed(() => cartGetters.getErrors(cart.value));

    return {
      cart,
      discounts,
      discountsAmount,
      hasDiscounts,
      totalItems,
      listIsHidden,
      products,
      totals,
      shippingPrice,
      removeItem,
      updateItemQty,
      cartGetters,
      characteristics: CHARACTERISTICS,
      errors,
      addBasePath
    };
  }
});
</script>

<style lang="scss" scoped>
.highlighted {
  box-sizing: border-box;
  width: 100%;
  background-color: var(--c-light);
  padding: var(--spacer-xl) var(--spacer-xl) 0;

  &:last-child {
    padding-bottom: var(--spacer-xl);
  }
}

.total-items {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: var(--spacer-xl);
}

.property {
  margin-bottom: var(--spacer-base);
}

.property-total {
  margin-top: var(--spacer-xl);
  padding-top: var(--spacer-lg);
  font-size: var(--font-size-xl);
  border-top: var(--c-white) 1px solid;
  --property-name-font-weight: var(--font-weight--semibold);
  --property-name-color: var(--c-text);
}

.characteristic {
  &:not(:last-child) {
    margin-bottom: var(--spacer-base);
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
          display: flex;
          color: #fff;
          text-align: center;
          justify-content: center;
          padding: 1rem 0rem;
          border-radius: 10px;
        }

  }

  #cart {
  box-sizing: border-box;
  display: flex;
  @include for-desktop {
    max-width: 1240px;
    margin: 0 auto;
  }

  .cart-content{
    flex: 100%;
    border: 1px solid #2F9393;
    padding: 1rem 10px;
    margin: 1rem 0px;

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
          display: flex;
          color: #fff;
          text-align: center;
          justify-content: center;
          padding: 1rem 0rem;
          border-radius: 10px;
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
  max-width: 100%;
  box-shadow: none;
  border: unset;
  justify-content: space-around;
  height: 200px;
  padding: 2rem 0rem;

  margin-bottom: 2rem;
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

.product-cart-title{
  font-size: 15px;
}

</style>
