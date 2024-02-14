<template>
  <div class="topbar">

    <template class="slider" style="width: 100% !important">
      <LazyHydrate when-visible>
      <SfCarousel class="carousel" :settings="{ peek: 16, breakpoints: { 1023: { peek: 0, perView: 3 } } }">
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
        <SfCarouselItem class="carousel__item">
          <span>Descuentos sobre descuentos</span>
        </SfCarouselItem>
        
        <SfCarouselItem class="carousel__item">
          <span>Promociones</span>
        </SfCarouselItem>
        <SfCarouselItem class="carousel__item">
          <span>Mayoristas</span>
        </SfCarouselItem>
        <SfCarouselItem class="carousel__item">
          <span>Despachos a todo chile</span>
        </SfCarouselItem>
      </SfCarousel>
    </LazyHydrate>
    </template>


  </div>
</template>

<script>
import { SfButton, SfTopBar,  SfCarousel, SfArrow} from '@storefront-ui/vue';
import LocaleSelector from './LocaleSelector';
import { computed } from '@nuxtjs/composition-api';
import LazyHydrate from 'vue-lazy-hydration';

export default {
  components: {
    SfTopBar,
    SfButton,
    LocaleSelector,
    SfCarousel,
    SfArrow
  },
  setup(props, context) {
    return {
      isMultiLang: computed(() => context.root.$i18n.locales ? context.root.$i18n.locales.length > 1 : false)
    };
  }
};

</script>
<style lang="scss" scoped>
.topbar {
  position: relative;
  background-color: #17ffe3;
  padding: 10px;
  z-index: 2;
  &__button {
    margin: 0 0 0 var(--spacer-xs);
  }

  .slider{
    width: 100%;
    max-width: 1225px;
    margin-left: auto;
    margin-right: auto;
  }

  .sf-top-bar__center{
    width: 100% !important;
  }
}

.sf-top-bar__center{
  width: 100%;
}

.carousel__item span{
  width: 25%;
  text-align: center;
}

.sf-carousel .glide__slide li{
  border-left: 5px solid #000;
  height: 5px;
}
.carousel__item li{
  border-left: 5px solid #000;
  height: 5px;
}
</style>
