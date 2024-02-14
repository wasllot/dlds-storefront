<template>
  <div>
    <SfHeader
      class="sf-header--has-mobile-search"
      :class="{'header-on-top': isSearchOpen}"
      :isNavVisible="isMobileMenuOpen"
    >
      <!-- TODO: add mobile view buttons after SFUI team PR -->
      <template #logo>
        <nuxt-link :to="localePath({ name: 'home' })" class="sf-header__logo">
          <SfImage :src="addBasePath('/icons/logo.svg')" alt="Vue Storefront Next" class="sf-header__logo-image"  />
        </nuxt-link>
      </template>
      <template #navigation>
        <HeaderCategoryNavigation :isMobile="isMobile" />
      </template>
      <template #header-icons>
        <div v-e2e="'header-icons'" class="sf-header__icons">
          <SfButton
            v-if="!validateAccountLogin"
            class="sf-button--pure sf-header__action"
            color="#2f9393"
            aria-label="Open account button"
            @click="handleAccountClick"
          >
            <SfIcon
              :icon="accountIcon"
              color="#2f9393"
              size="29px"
            />
          ¡HOLA!<br>Inicia sesión
          </SfButton>
          <SfButton
            v-else
            class="sf-button--pure sf-header__action"
            color="#2f9393"
            aria-label="Ir a mi cuenta"
            @click="handleAccountClick"
          >
            <SfIcon
              :icon="accountIcon"
              color="#2f9393"
              size="29px"
            />
          Mi cuenta
          </SfButton>
          <SfButton
          
            class="sf-button--pure sf-header__action"
            aria-label="Toggle cart sidebar"
            @click="toggleCartSidebar"
          >
            <SfIcon
              class="sf-header__icon"
              icon="empty_cart"
              color="#2f9393"
              size="29px"
            />
             Mi Carrito
            <SfBadge v-if="cartTotalItems" class="sf-badge--number cart-badge">{{cartTotalItems}}</SfBadge>
          </SfButton>
        </div>
      </template>
      <template #search>
        <SfSearchBar
          ref="searchBarRef"
          placeholder="Hola, ¿Qué estás buscando?"
          color="#2f9393"
          aria-label="Search"
          class="sf-header__search"
          :value="term"
          @input="handleSearch"
          @keydown.enter="handleSearch($event)"
          @focus="isSearchOpen = true"
          @keydown.esc="closeSearch"
          v-click-outside="closeSearch"
        >
          <template #icon>
            <SfButton
              v-if="!!term"
              aria-label="Close search"
              class="sf-search-bar__button sf-button--pure"
              @click="closeOrFocusSearchBar"
            >
              <span class="sf-search-bar__icon">
                <SfIcon color="#2f9393" size="18px" icon="cross" />
              </span>
            </SfButton>
            <SfButton
              v-else
              aria-label="Open search"
              class="sf-search-bar__button sf-button--pure"
              @click="isSearchOpen ? isSearchOpen = false : isSearchOpen = true"
            >
              <span class="sf-search-bar__icon">
                <SfIcon color="#2f9393" size="20px" icon="search" />
              </span>
            </SfButton>
          </template>
        </SfSearchBar>
      </template>

    </SfHeader>
    <HeaderNav />
    <SearchResults
      :visible="isSearchOpen"
      :result="result"
      :term="term"
      @close="closeSearch"
      @removeSearchResults="removeSearchResults"
    />
    <SfOverlay :visible="isSearchOpen" />
  </div>
</template>

<script>
import { SfHeader, SfImage, SfIcon, SfButton, SfBadge, SfSearchBar, SfOverlay } from '@storefront-ui/vue';
import { useUiState } from '~/composables';
import { useCart, useUser, cartGetters, useFacet, facetGetters } from '@vue-storefront/prestashop';
import { computed, ref, watch, onBeforeUnmount, useRouter } from '@nuxtjs/composition-api';
import { useUiHelpers } from '~/composables';
import LocaleSelector from './LocaleSelector';
import SearchResults from '~/components/SearchResults';
import HeaderNav from './HeaderNav';

import HeaderCategoryNavigation from './HeaderCategoryNavigation';

import { clickOutside } from '@storefront-ui/vue/src/utilities/directives/click-outside/click-outside-directive.js';
import {
  mapMobileObserver,
  unMapMobileObserver
} from '@storefront-ui/vue/src/utilities/mobile-observer.js';
import debounce from 'lodash.debounce';
import { addBasePath } from '@vue-storefront/core';

export default {
  components: {
    SfHeader,
    SfImage,
    LocaleSelector,
    SfIcon,
    SfButton,
    SfBadge,
    SfSearchBar,
    SearchResults,
    SfOverlay,
    HeaderNav,
    HeaderCategoryNavigation
  },
  directives: { clickOutside },
  setup(props, { root }) {
    const router = useRouter();
    const { toggleCartSidebar, toggleWishlistSidebar, toggleLoginModal, isMobileMenuOpen } = useUiState();
    const { setTermForUrl, getFacetsFromURL } = useUiHelpers();
    const { isAuthenticated } = useUser();
    const { cart } = useCart();
    const term = ref(getFacetsFromURL().phrase);
    const isSearchOpen = ref(false);
    const searchBarRef = ref(null);
    const isMobile = ref(mapMobileObserver().isMobile.get());

    const { result: productResult, search: productSearch } = useFacet();
    const result = ref(null);

    const cartTotalItems = computed(() => {
      const count = cartGetters.getTotalItems(cart.value);
      return count ? count.toString() : null;
    });

    const accountIcon = computed(() => isAuthenticated.value ? 'profile_fill' : 'profile');

    const validateAccountLogin = async () => {
      if (isAuthenticated.value) {
       return true;
      }

      return false;
    };

    const handleAccountClick = async () => {
      if (isAuthenticated.value) {
        const localeAccountPath = root.localePath({ name: 'my-account' });
        return router.push(localeAccountPath);
      }

      toggleLoginModal();
    };

    const closeSearch = () => {
      const wishlistClassName = 'sf-product-card__wishlist-icon';
      const isWishlistIconClicked = event.path.find(p => wishlistClassName.search(p.className) > 0);
      if (isWishlistIconClicked || !isSearchOpen.value) return;

      term.value = '';
      isSearchOpen.value = false;
    };

    const handleSearch = debounce(async (paramValue) => {
      if (!paramValue.target) {
        term.value = paramValue;
      } else {
        term.value = paramValue.target.value;
      }

      await productSearch({
        type: 'instant-search',
        term: term.value
      });

      result.value = facetGetters.getProducts(productResult.value);

    }, 200);

    const closeOrFocusSearchBar = () => {
      if (isMobile.value) {
        return closeSearch();
      } else {
        term.value = '';
        return searchBarRef.value.$el.children[0].focus();
      }
    };

    watch(() => term.value, (newVal, oldVal) => {
      const shouldSearchBeOpened = (!isMobile.value && term.value.length > 0) && ((!oldVal && newVal) || (newVal.length !== oldVal.length && isSearchOpen.value === false));
      if (shouldSearchBeOpened) {
        isSearchOpen.value = true;
      }
    });

    const removeSearchResults = () => {
      result.value = null;
    };

    onBeforeUnmount(() => {
      unMapMobileObserver();
    });

    return {
      accountIcon,
      cartTotalItems,
      handleAccountClick,
      validateAccountLogin,
      toggleCartSidebar,
      toggleWishlistSidebar,
      setTermForUrl,
      term,
      isSearchOpen,
      closeSearch,
      handleSearch,
      result,
      closeOrFocusSearchBar,
      searchBarRef,
      isMobile,
      isMobileMenuOpen,
      removeSearchResults,
      addBasePath,
      isMultiLang: computed(() => root.$i18n.locales ? root.$i18n.locales.length > 1 : false)
    };
  }
};
</script>

<style lang="scss" scoped>

.sf-header__logo{
  width: 20%;
}
.sf-header {
  --header-padding:  var(--spacer-sm);
  @include for-desktop {
    --header-padding: 0;
  }
  &__logo-image {
    height: 100%;
  }
}
.header-on-top {
  z-index: 2;
}
.nav-item {
  --header-navigation-item-margin: 0 var(--spacer-base);
  .sf-header-navigation-item__item--mobile {
    display: none;
  }
}

.cart-badge {
  position: absolute;
  bottom: 40%;
  left: 15%;
}

.sf-image{
  width: 100%;
}

.sf-header{

  &__action{
    text-align: left;
    font-size: 14px;
    color: #2f9393;
  }

  &__navigation{
    margin-left: 0 !important;
  }
  
}

.sf-search-bar{
  flex: 50%;
  margin-right: 20px;
    margin-left: 20px;
}



</style>
