<template>
  <div class="sf-header__navigation desktop" v-if="!isMobile">
    <SfButton class="sf-button--pure sf-header__action">
      <template>
        <SfIcon
          icon="cross"
          size="lg"
          color="#2f9393"
          viewBox="0 0 24 24"
          :coverage="1"
          v-if="menuMouseStatus['Clothes']"
          @click='mouseLeave("Clothes")'

          
        />
        <SfIcon
          icon="list"
          size="lg"
          color="#2f9393"
          viewBox="0 0 24 24"
          :coverage="1"
          v-else
          @click='mouseOpen("Clothes")'
          
        />
      </template>
    </SfButton>
    <SfHeaderNavigationItem
      v-for="(category, index) in categories"
      :key="index"
      class="nav-item"
      v-e2e="`app-header-url_${category.slug}`"
      label="Categorías"
      color="#2f9393"
      link="#"
      @click='mouseOver(category)'
      v-if="index == 0"
    >
      <SfMegaMenu
        :visible="menuMouseStatus[category.label]"
        class="menu"
        isAbsolute
        color="#2f9393"
      >
          <div class="menu__wrapper-items" key="items">
            <SfMegaMenuColumn :title="$t('Product suggestions')" class="sf-mega-menu-column--pined-content-on-mobile menu__items">
              <template #title="{title}">
                <SfScrollable class="items--desktop desktop-only" show-text="" hide-text="">
                  <div class="items-listing container">
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/vaporizacion.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Vaporización" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/iluminacion.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Iluminación" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/fertilizantes.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Fertilizantes" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/sustratos.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Sustratos" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/carpas.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Carpas" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/climatizacion.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Climatización" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/herramientas.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Herramientas y Accesorios" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/medicion.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Medición" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/bongs.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Bongs" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/papelillo.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Papelillos" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/limpieza.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Limpieza" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/extraccion.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Extracción" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/pipas.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Pipas" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                    <div class="menu-item-img">
                      <img loading="lazy" src="/icons/SVG/menu/instrumentos.svg" srcset="" sizes=""  class="sf-image-loaded" style="width: 30px; height: 30px">
                      <SfMenuItem label="Instrumentos" :link="localePath(`/c/grow/vaporizadores`)"
                      />
                    </div>
                  </div>
                </SfScrollable>
              </template>
            </SfMegaMenuColumn>
            <div class="action-buttons smartphone-only">
              <SfButton class="action-buttons__button color-light" @click="$emit('close')">{{ $t('Cancel') }}</SfButton>
            </div>
          </div>
      </SfMegaMenu>
    </SfHeaderNavigationItem>
  </div>
  <SfModal v-else :visible="isMobileMenuOpen">
    <SfAccordion
      v-e2e="'categories-accordion'"
      :show-chevron="true"
    >
      <AccordionItem
        v-for="(cat, i) in categories"
        :key="i"
        :hasItems="cat.children.length"
      >
        <template #title>
          <nuxt-link
            :to="localePath(`/c/${cat.slug}`)"
            @click.native="toggleMobileMenu"
          >
            {{ cat.label }}
          </nuxt-link>
        </template>

        <template #default>
          <SfList class="list">
            <SfListItem
              class="list__item"
              v-for="(subCat, j) in cat.children"
              :key="j"
            >
              <nuxt-link
                :to="localePath(`/c/${subCat.slug}`)"
                :class="subCat.isCurrent ? 'sidebar--cat-selected' : ''"
                @click.native="toggleMobileMenu">
              <SfMenuItem
                :count="subCat.count || ''"
                :label="subCat.label"
              />
              </nuxt-link>
            </SfListItem>
          </SfList>
        </template>
      </AccordionItem>
    </SfAccordion>
  </SfModal>
</template>

<script>
import { SfMenuItem, SfModal, SfMegaMenu, SfAccordion, SfList, SfScrollable, SfButton, SfIcon, SfIconMenu, SfIconBase} from '@storefront-ui/vue';

import { useUiState } from '~/composables';
import {
  useBootstrap
} from '@vue-storefront/prestashop';
import { computed } from '@nuxtjs/composition-api';
import { addBasePath } from '@vue-storefront/core';
import AccordionItem from '~/components/AccordionItem';

export default {
  name: 'HeaderCategoryNavigation',
  components: {
    SfList,
    SfMenuItem,
    SfModal,
    SfIcon,
    SfMegaMenu,
    SfAccordion,
    AccordionItem,
    SfScrollable,
    SfButton
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
        console.log(category.label)
      }
    };
    const mouseLeave = (category) => {
      // menuMouseStatus.value[category.label] = false;
      menuMouseStatus.value[category] = false;

    };

    const mouseOpen = (category) => {
      // menuMouseStatus.value[category.label] = false;
      menuMouseStatus.value[category] = true;

    };

    return {
      categories,
      isMobileMenuOpen,
      toggleMobileMenu,
      mouseOver,
      mouseLeave,
      mouseOpen,
      menuMouseStatus,
      products,
      addBasePath
    };
  }
};
</script>

<style lang="scss" scoped>


header{
  margin: 0 !important;
  max-width: 96% !important;
}
.menu-item-img{
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 10px;
}

.menu-item-img img{
  padding-right: 10px;
}
.menu-item-img a{
  font-size: 25px;
  padding-right: 20px;

}
.sf-mega-menu{
  width: 30% ;
  height: 80vh;
}


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
  --mega-menu-height: 30px;
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
    --scrollable-max-height: 70vh;
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
  margin-left: 10%;
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

.sf-header-navigation-item{
  &__link{
    color: #2f9393;
  }
}
</style>
