<template>
  <div class="container_menu_bottom">
    <div class="sf-header__navigation desktop menu_bottom" v-if="!isMobile">
        <nuxt-link :to="localePath({ name: 'productos-rebajados' })" class="bold">
          <img loading="lazy" src="/icons/svg/icon-fire.svg" srcset="" sizes="" width="20" height="20" class="sf-image-loaded" style="width: 20px;">HOT SALE</nuxt-link>
          
          <div class="separator"></div>

          <nuxt-link :to="localePath({ name: 'productos-nuevos' })" class="bold"><img loading="lazy" src="/icons/svg/icon-fly.svg" srcset="" sizes="" width="20" height="20" class="sf-image-loaded" style="width: 20px;">
            NUEVOS PRODUCTOS
          </nuxt-link>

          <div class="separator"></div>

          <nuxt-link :to="localePath({ name: 'nosotros' })" class="">
            nosotros
          </nuxt-link>
          
          <div class="separator"></div>
          <nuxt-link :to="localePath({ name: 'como-comprar' })" class="">
            ¿Cómo comprar?
          </nuxt-link>
          
          <div class="separator"></div>

          <nuxt-link :to="localePath({ name: 'home' })" class="">
            Términos y condiciones
          </nuxt-link>
          
          <div class="separator"></div>
          <nuxt-link :to="localePath({ name: 'home' })" class="">
            Contacto
          </nuxt-link>
          
    </div>
  </div>


</template>

<script>
import { SfMenuItem, SfModal, SfMegaMenu, SfAccordion, SfList, SfScrollable, SfButton, SfImage } from '@storefront-ui/vue';

import { useUiState } from '~/composables';
import {
  useBootstrap
} from '@vue-storefront/prestashop';
import { computed } from '@nuxtjs/composition-api';
import { addBasePath } from '@vue-storefront/core';
import AccordionItem from '~/components/AccordionItem';

export default {
  name: 'HeaderNavigation',
  components: {
    SfList,
    SfMenuItem,
    SfModal,
    SfMegaMenu,
    SfAccordion,
    AccordionItem,
    SfScrollable,
    SfButton,
    SfImage
  },
  props: {
    isMobile: {
      type: Boolean,
      default: false
    },
    result: {
      type: Array
    }
  },
  setup(props) {
    const products = computed(() => props.result);
    const {
      menuItems: categories,
      menuMouseStatus
    } = useBootstrap();

    const { isMobileMenuOpen, toggleMobileMenu } = useUiState();

    const mouseOver = (category) => {
      if (category.children.length > 0) {
        // context.root.$emit('hoverItem', category);
        menuMouseStatus.value[category.label] = true;
      }
    };
    const mouseLeave = (category) => {
      menuMouseStatus.value[category.label] = false;
    };

    return {
      categories,
      isMobileMenuOpen,
      toggleMobileMenu,
      mouseOver,
      mouseLeave,
      menuMouseStatus,
      products,
      addBasePath
    };
  }
};
</script>

<style lang="scss" scoped>
.sf-header-navigation-item {
  ::v-deep &__item--mobile {
    display: block;
  }
}
.sf-modal {
  ::v-deep &__bar {
    display: none;
  }
  ::v-deep &__content {
    padding: var(--modal-content-padding, var(--spacer-base) 0);
  }
}

.sf-menu-item{
  width: 100%;
}
.menu {
  position: absolute;
  right: 0;
  left: 0;
  z-index: 3;
  --mega-menu-column-header-margin: var(--spacer-sm) 0 var(--spacer-xl);
  --mega-menu-content-padding: 0;
  --mega-menu-height: auto;
  @include for-desktop {
    --mega-menu-content-padding: var(--spacer-xl) 0;
  }
  &__wrapper-items {
    display: flex;
    flex-direction: column;
    @include for-desktop {
      flex-direction: row;
      flex: 1;
    }
  }
  &__categories {
    flex: 0 0 220px;
  }
  &__items {
    flex: 1
  }
  &__header {
    margin-left: var(--spacer-sm);
  }
  ::v-deep .sf-bar {
    display: none;
  }
  ::v-deep .sf-mega-menu-column__header {
    display: none;
    @include for-desktop {
      display: flex;
    }
  }
}
.items {
  &--desktop {
    --scrollable-max-height: 35vh;
  }
  &--mobile {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    background: var(--c-white);
    padding: var(--spacer-base) var(--spacer-sm);
    --product-card-max-width: 9rem ;
  }
}
.see-all {
  padding: var(--spacer-xl) 0 0 var(--spacer-sm);
}
.action-buttons {
  padding: var(--spacer-xl) var(--spacer-sm) var(--spacer-3xl);
  background: var(--c-white);
  width: 100%;
  &__button {
    width: calc(100% - 32px);
  }
}
.items-listing {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  margin-left: var(--spacer-xl);
  &.container {
    margin-left: calc( var(--spacer-2xl) + 50px );
  }
}
.item-card {
  margin: var(--spacer-sm) 0;
  @include for-desktop {
    margin: var(--spacer-2xs) 0;
  }
}

.before-items {
  box-sizing: border-box;
  padding: var(--spacer-base) var(--spacer-sm) var(--spacer-2xl);
  width: 100%;
  text-align: center;
  @include for-desktop {
    padding: 0;
  }
  &__picture {
    --image-width: 230px;
    margin-top: var(--spacer-2xl);
    @include for-desktop {
      --image-width: 18.75rem;
      margin-top: var(--spacer-base);
    }
  }
  &__paragraph {
    font-family: var(--font-family--primary);
    font-weight: var(--font-weight--normal);
    font-size: var(--font-size--base);
    color: var(--c-text-muted);
    margin: 0;
    @include for-desktop {
      font-size: var(--font-size--lg);
    }
    &:first-of-type {
      margin: var(--spacer-xl) auto var(--spacer-xs);
    }
  }
  &__button {
    margin: var(--spacer-xl) auto;
    width: 100%;
  }
}
.invisible {
  visibility: hidden !important;
}

.container_menu_bottom{
  background: #0caa91;

}
.menu_bottom{
  width: 100%;
  max-width: 1225px;
  margin-right: auto;
  margin-left: auto;
  height: 40px;
  display: flex;
  align-items: center;

  a{
    display: flex;
    align-items: center;
    justify-content: center;
    color: #fff;
    padding-right: 10px;
    padding-left: 10px;

  }

  .bold{
      font-weight: bolder;

    }
  .separator{
    border-right: 2px solid #fff;
    height:  10px;
    margin: 0 5px;
  }
  img{
    width: 30px !important;
    padding-right: 0;
    padding-left: 0;

  }

}
</style>
