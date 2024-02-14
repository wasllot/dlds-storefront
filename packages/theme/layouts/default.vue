<template>
  <div>
    <LazyHydrate when-visible>
      <TopBarSlider class="desktop-only" />
    </LazyHydrate>

    <AppHeader />

    <div id="layout">
      <nuxt :key="$route.fullPath"/>

      <BottomNavigation />
      <CartSidebar />
      <WishlistSidebar />
      <LoginModal />
      <Notification />
    </div>
    <LazyHydrate when-visible>
      <AppFooter />
    </LazyHydrate>
  </div>
</template>

<script>
import AppHeader from '~/components/AppHeader.vue';
import BottomNavigation from '~/components/BottomNavigation.vue';
import AppFooter from '~/components/AppFooter.vue';
import TopBar from '~/components/TopBar.vue';
import TopBarSlider from '~/components/TopBarSlider.vue';
import CartSidebar from '~/components/CartSidebar.vue';
import WishlistSidebar from '~/components/WishlistSidebar.vue';
import LoginModal from '~/components/LoginModal.vue';
import LazyHydrate from 'vue-lazy-hydration';
import Notification from '~/components/Notification';
import { onSSR } from '@vue-storefront/core';
import { useRoute } from '@nuxtjs/composition-api';
import { useCart, useStore, useUser, useWishlist, useBootstrap } from '@vue-storefront/prestashop';

export default {
  name: 'DefaultLayout',

  components: {
    LazyHydrate,
    TopBar,
    TopBarSlider,
    AppHeader,
    BottomNavigation,
    AppFooter,
    CartSidebar,
    WishlistSidebar,
    LoginModal,
    Notification
  },

  setup() {
    const route = useRoute();
    const { load: loadStores } = useStore();
    const { load: loadUser } = useUser();
    const { load: loadCart } = useCart();
    const { load: loadWishlist } = useWishlist();

    const {
      boot: boot
    } = useBootstrap();

    onSSR(async () => {
      await Promise.all([
        boot(),
        loadStores(),
        loadUser(),
        loadCart(),
        loadWishlist()
      ]);
    });

    return {
      route
    };
  }
};
</script>

<style lang="scss">
@import "~@storefront-ui/vue/styles";

#layout {
  box-sizing: border-box;
  @include for-desktop {
    max-width: 100%;
    margin: auto;
  }
}

.no-scroll {
  overflow: hidden;
  height: 100vh;
}

// Reset CSS
html {
  width: auto;
  @include for-mobile {
    overflow-x: hidden;
  }
}
body {
  overflow-x: hidden;
  color: var(--c-text);
  font-size: var(--font-size--base);
  font-family: var(--font-family--primary);
  margin: 0;
  padding: 0;
}
a {
  text-decoration: none;
  color: var(--c-link);
  &:hover {
    color: var(--c-link-hover);
  }
}
h1 {
  font-family: var(--font-family--secondary);
  font-size: var(--h1-font-size);
  line-height: 1.6;
  margin: 0;
}
h2 {
  font-family: var(--font-family--secondary);
  font-size: var(--h2-font-size);
  line-height: 1.6;
  margin: 0;
}
h3 {
  font-family: var(--font-family--secondary);
  font-size: var(--h3-font-size);
  line-height: 1.6;
  margin: 0;
}
h4 {
  font-family: var(--font-family--secondary);
  font-size: var(--h4-font-size);
  line-height: 1.6;
  margin: 0;
}

.sf-overlay{
  background: #1d1d1b4f !important;
}


.sf-modal__close {
  margin-top: auto;
  margin-bottom: auto;
  padding-top: 4%;
  padding-right: 8%;
}

.sf-modal__container{
  padding: 1rem;
  width: 33rem;
  border-radius: 25px;
}

.sf-modal__container .sf-input input{
  background: #F2F2F2;
  border: unset;
  border-radius: 15px;
}

.sf-modal__container .sf-input input::placeholder{
  padding: 1rem;
}

.form form span .form__element input{
  background: #F2F2F2;
    padding: 13px;
    border: unset;
    border-radius: 15px;
}

.form form span .form__element .sf-input__wrapper{
  margin: 0;
}


.sf-header__navigation{
  margin: 0;
}

.sf-search-bar{
  max-width: 500px;
}

.sf-header__actions{
  justify-content: space-around;
}

.items-listing.container{
  margin-left: 10px;
}

@media(max-width: 1020px){
  .sf-modal__container{
    width: 100%;
  }
}

.carousel-counter .carousel-counter-logo{
  width: 65%;
  justify-content: start;
  display: flex;
    
}

.carousel-counter .carousel-counter-logo img{
    width: 30%;
    text-align: start;
    margin-top: 1rem;
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

#category .sf-accordion {
  border-top: 1px solid #9E9E9E;
}

#category .sf-accordion .sf-accordion-item{
  border-bottom: none;
}

#category .sf-accordion .sf-accordion-item button{
  padding: 2px;
}

#category .sf-chevron--right .sf-chevron__bar--right, #category .sf-chevron--right .sf-chevron__bar--left{
  opacity: 0.2;
}

.cart-content .sf-collected-product__quantity-wrapper{
  display: none;
}

.cart-content .sf-collected-product__main{
  display: none;
}

img{
    outline-color: unset !important;
    outline-width: unset !important;
    outline-style: unset !important;
}

.cart-content .sf-collected-product__quantity-selector{
  background: #fff;
    border: 2px solid #0caa91;
    border-radius: 25px;
    width: 80%;
}

.cart-content .sf-quantity-selector__button, .cart-content .sf-input input[type=number]{
    font-size: 24px;
    color: #0caa91;
    font-weight: bolder;
}

.sf-steps__header{
    height: 60px;
    margin-top: -60px;
    background: #009688;
}

.sf-steps__title{
  color: #fff;
}

.sf-collected-product__remove--circle-icon{
  display: none !important;
}

#cart .sf-image{
  width: 60%;
}

#cart .sf-collected-product__image{
  background: none;
    justify-content: center;
    align-items: center;
    display: flex;
}

.sf-accordion-item__header.is-open {
    --accordion-item-header-border-width: 0;
    --accordion-item-header-color: #0caa91;
    --chevron-color: #0caa91;
}

#category .sf-accordion-item__content{
  padding: 1px 5px;
}

.sf-product-card__title{
  font-weight: bold;
    color: #808080;
    font-size: 12px;
}

.sf-price__regular{
  color: #009688;
    font-weight: bold;
    font-size: 15px;
}

.sf-menu-item.is-active{
  color: #009688 !important;
}

.form__button{
  background: #009688;
}
</style>
