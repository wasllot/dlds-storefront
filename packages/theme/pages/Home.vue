<template>
  <div>
    <LazyHydrate when-visible>
      <SfHero class="hero">
        <SfHeroItem
          v-for="(hero, i) in heroes"
          :key="i"
          :title="hero.title"
          :subtitle="hero.subtitle"
          :background="hero.background"
          :image="hero.image"
          :class="hero.className"
        />
      </SfHero>
    </LazyHydrate>
    <div id="home">


          <template class="slider slider-cats" style="width: 100% !important">
        <LazyHydrate when-visible>
        <SfCarousel class="carousel carousel-cats" :settings="{ peek: 16, breakpoints: { 1023: { peek: 0, perView: 5 } } }">

          <SfCarouselItem class="carousel__item">
            <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/icons/svg/climatizacion.svg" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              <br>
              CLIMATIZACIÓN
            </nuxt-link>
          </SfCarouselItem>
          <SfCarouselItem class="carousel__item">
            <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/icons/svg/herramientas-y-accesorios.svg" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              <br>
              HERRAMIENTAS Y ACCESORIOS
            </nuxt-link>
          </SfCarouselItem>
            <SfCarouselItem class="carousel__item">
            <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/icons/svg/medicion.svg" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              <br>
              MEDICIÓN
            </nuxt-link>
          </SfCarouselItem>
          <SfCarouselItem class="carousel__item">
            <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/icons/svg/bongs.svg" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              <br>
              BONGS
            </nuxt-link>
          </SfCarouselItem>
          <SfCarouselItem class="carousel__item">
            <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/icons/svg/papelillos.svg" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              <br>
              PAPELILLOS
            </nuxt-link>
          </SfCarouselItem>
          <SfCarouselItem class="carousel__item">
            <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/icons/svg/limpieza.svg" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              <br>
              LIMPIEZA
            </nuxt-link>
          </SfCarouselItem>
          <SfCarouselItem class="carousel__item">
            <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/icons/svg/extraccion.svg" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              <br>
              EXTRACCIÓN
            </nuxt-link>
          </SfCarouselItem>
          <SfCarouselItem class="carousel__item">
            <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/icons/svg/pipas.svg" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              <br>
              PIPAS
            </nuxt-link>
          </SfCarouselItem>
          <SfCarouselItem class="carousel__item">
            <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/icons/svg/instrumentos.svg" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              <br>
              INSTRUMENTOS
            </nuxt-link>
          </SfCarouselItem>
        </SfCarousel>
      </LazyHydrate>
      </template>

      <LazyHydrate when-visible>
        <div class="categories ">
          <template v-for="item in banners">

            <div class="category-item">
              <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" :src="item.image" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
                <span>{{item.title}}</span>
              </nuxt-link>
            </div>
            
          </template>
        </div>
      </LazyHydrate>

      <LazyHydrate when-visible>
        <div class="similar-products">
          <SfHeading title="- Productos destacados - " :level="2"/>
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

      <LazyHydrate when-visible>
        <div class="carousel-counter">
          <div class="carousel-counter-col counter">
            <h2 class="carousel-counter-title">¡Aprovecha el descuento solo por unas horas!</h2>
            <hr>
            <div class="carousel-counter-counter">

              <div id="countdown">
                <ul>
                  <li><span id="hours">33</span>Horas</li>
                  <div class="timer-separator">:</div>
                  <li><span id="minutes">22</span>Minutos</li>
                  <div class="timer-separator">:</div>
                  <li><span id="seconds">11</span>Segundos</li>
                </ul>
              </div>

            </div>
            <hr>
            <div class="carousel-counter-logo">
              <img src="/icons/logo.png"/>
            </div>
          </div>
          <div class="carousel-counter-col">
            <SfCarousel class="carousel" :settings="{ peek: 0,  perView: 1, breakpoints: { 1023: { peek: 0, perView: 1 } } }">
              <SfCarouselItem class="carousel__item carousel-item-counter" v-for="(product, i) in products" :key="i">
                <div class="carousel-item-counter-image">
                  <img :src="productGetters.getCoverImage(product)"/>
                </div>
                <div class="carousel-item-counter-content" style="text-align: left;padding: 1rem;">
                    <div class="carousel-item-discount">43%</div>
                    <div class="carousel-item-brand">Brand</div>
                    <div class="carousel-item-name">{{ productGetters.getName(product) }}</div>
                    <div class="carousel-item-specialprice">{{ $n(productGetters.getPrice(product).regular, 'currency') }}</div>
                    <div class="carousel-item-normalprice">Normal: {{ $n(productGetters.getPrice(product).regular, 'currency') === $n(productGetters.getPrice(product).special, 'currency')? '': $n(productGetters.getPrice(product).special, 'currency') }}</div>
                    <a :href="localePath(`/p/${productGetters.getId(product)}/${productGetters.getSlug(product)}`)" class="carousel-item-button">¡Lo quiero!</a>
                </div>
                <!-- <SfProductCard
                  :title="productGetters.getName(product)"
                  :image="productGetters.getCoverImage(product)"
                  :regular-price="$n(productGetters.getPrice(product).regular, 'currency')"
                  :special-price="$n(productGetters.getPrice(product).regular, 'currency') === $n(productGetters.getPrice(product).special, 'currency')? '': $n(productGetters.getPrice(product).special, 'currency')"
                  :show-add-to-cart-button="true"
                  :link="localePath(`/p/${productGetters.getId(product)}/${productGetters.getSlug(product)}`)"
                  class="carousel__item__product"
                  @click:add-to-cart="HandleAddToCart({ product, quantity:1 })"
                /> -->
              </SfCarouselItem>
            </SfCarousel>
          </div>
        </div>
      </LazyHydrate>

      <LazyHydrate when-visible>
        <div class="similar-products">
          <SfHeading title="- Promos del mes - " :level="2"/>
          <nuxt-link :to="localePath('/c/women')" class="smartphone-only">
            {{ $t('See all') }}
          </nuxt-link>
        </div>
      </LazyHydrate>

      <LazyHydrate when-visible>
        <div class="categories promos">

            <div class="category-item">
              <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/homepage/promo-1.png" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              </nuxt-link>
            </div>
            
            <div class="category-item">
              <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/homepage/promo-2.png" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              </nuxt-link>
            </div>
            
            <div class="category-item">
              <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/homepage/promo-3.png" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              </nuxt-link>
            </div>
            
            <div class="category-item">
              <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/homepage/promo-4.png" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              </nuxt-link>
            </div>
            
            <div class="category-item">
              <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/homepage/promo-5.png" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              </nuxt-link>
            </div>
            
            <div class="category-item">
              <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/homepage/promo-6.png" srcset="" sizes=""  class="sf-image-loaded" style="height: 120px; width: auto">
              </nuxt-link>
            </div>
            
        </div>
      </LazyHydrate>
      <br><br>
      
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

      <br><br>
      <LazyHydrate when-visible>
        <div class="similar-products">
          <SfHeading title="- Los más vendidos - " :level="2"/>
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
          <SfCarouselItem class="carousel__item" v-for="(product, i) in products" :key="i" style="margin-right: 10px;">
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

  </div>

</template>
<script>
import {
  SfHero,
  SfBanner,
  SfCallToAction,
  SfSection,
  SfCarousel,
  SfProductCard,
  SfImage,
  SfBannerGrid,
  SfHeading,
  SfArrow,
  SfButton
} from '@storefront-ui/vue';
import { ref, useContext } from '@nuxtjs/composition-api';
import InstagramFeed from '~/components/InstagramFeed.vue';
import NewsletterModal from '~/components/NewsletterModal.vue';
import OffersCounter from '~/components/OffersCounter.vue';
import LazyHydrate from 'vue-lazy-hydration';
import { useUiState } from '../composables';
import cacheControl from './../helpers/cacheControl';
import { onSSR, addBasePath } from '@vue-storefront/core';
import { computed } from '@nuxtjs/composition-api';
import { useUiNotification } from '~/composables';

import {
  useProduct,
  productGetters,
  useCart
} from '@vue-storefront/prestashop';

export default {
  name: 'Home',
  setup() {
    const { $config } = useContext();
    const { toggleNewsletterModal } = useUiState();
    const products = ref([
      {
        title: 'Cream Beach Bag',
        image: addBasePath('/homepage/productA.webp'),
        price: { regular: '50.00 $' },
        rating: { max: 5, score: 4 },
        isInWishlist: true
      },
      {
        title: 'Cream Beach Bag 2',
        image: addBasePath('/homepage/productB.webp'),
        price: { regular: '50.00 $' },
        rating: { max: 5, score: 4 },
        isInWishlist: false
      },
      {
        title: 'Cream Beach Bag 3',
        image: addBasePath('/homepage/productC.webp'),
        price: { regular: '50.00 $' },
        rating: { max: 5, score: 4 },
        isInWishlist: false
      },
      {
        title: 'Cream Beach Bag RR',
        image: addBasePath('/homepage/productA.webp'),
        price: { regular: '50.00 $' },
        rating: { max: 5, score: 4 },
        isInWishlist: false
      },
      {
        title: 'Cream Beach Bag',
        image: addBasePath('/homepage/productB.webp'),
        price: { regular: '50.00 $' },
        rating: { max: 5, score: 4 },
        isInWishlist: false
      },
      {
        title: 'Cream Beach Bag',
        image: addBasePath('/homepage/productC.webp'),
        price: { regular: '50.00 $' },
        rating: { max: 5, score: 4 },
        isInWishlist: false
      },
      {
        title: 'Cream Beach Bag',
        image: addBasePath('/homepage/productA.webp'),
        price: { regular: '50.00 $' },
        rating: { max: 5, score: 4 },
        isInWishlist: false
      },
      {
        title: 'Cream Beach Bag',
        image: addBasePath('/homepage/productB.webp'),
        price: { regular: '50.00 $' },
        rating: { max: 5, score: 4 },
        isInWishlist: false
      }
    ]);
    const heroes = [
      {
        title: '',
        subtitle: '',
        background: '#eceff1',
        image: addBasePath('/homepage/banner-1.jpg')
      },
      {
        title: '',
        subtitle: '',
        background: '#efebe9',
        image: addBasePath('/homepage/banner-2.jpg'),
        className:
          'sf-hero-item--position-bg-top-left sf-hero-item--align-right'
      }
    ];
    const banners = [
      {
        slot: 'banner-A',
        subtitle: '',
        title: 'GROW',
        description:
          '',
        buttonText: '',
        image:  addBasePath('/homepage/grow.png'),
        class: 'sf-banner--slim banner__tshirt',
        link: $config.theme.home.bannerA.link
      },
      {
        slot: 'banner-B',
        subtitle: '',
        title: 'SMOKE',
        description:
          '',
        buttonText: '',
        image: addBasePath('/homepage/smoke.png'),
        class: 'sf-banner--slim banner__tshirt',
        link: $config.theme.home.bannerB.link
      },
      {
        slot: 'banner-C',
        subtitle: '',
        title: 'SEMILLAS',
        image: addBasePath('/homepage/semillas.png'),
        class: 'sf-banner--slim banner__tshirt',
        link: $config.theme.home.bannerC.link
      },
      {
        slot: 'banner-D',
        subtitle: '',
        title: 'TABAQUERIA',
        image: addBasePath('/homepage/tabaqueria.png'),
        class: 'sf-banner--slim banner__tshirt',
        link: $config.theme.home.bannerD.link
      },
      {
        slot: 'banner-E',
        subtitle: '',
        title: 'LIFESTYLE',
        image: addBasePath('/homepage/lifestyle.png'),
        class: 'sf-banner--slim banner__tshirt',
        link: '#'
      },
      {
        slot: 'banner-F',
        subtitle: '',
        title: 'EXTRACCIÓN',
        image: addBasePath('/homepage/extraccion.png'),
        class: 'sf-banner--slim',
        link: '#'
      }
    ];

    const onSubscribe = (emailAddress) => {
      console.log(`Email ${emailAddress} was added to newsletter.`);
      toggleNewsletterModal();
    };

    const toggleWishlist = (index) => {
      products.value[index].isInWishlist = !products.value[index].isInWishlist;
    };

    const {
      products: featureProducts,
      search: productsSearch,
      loading: productsLoading
    } = useProduct('relatedProducts');

    const { send: sendNotification } = useUiNotification();
    const { addItem: addItemToCart, isInCart } = useCart();

    onSSR(async () => {
      await productsSearch({ featured: true });
    });

    const timer = () => {

        const second = 1000,
          minute = second * 60,
          hour = minute * 60,
          day = hour * 24;

        //I'm adding this section so I don't have to keep updating this pen every year :-)
        //remove this if you don't need it
        let today = new Date(),
            dd = String(today.getDate()).padStart(2, "0"),
            mm = String(today.getMonth() + 1).padStart(2, "0"),
            yyyy = today.getFullYear(),
            nextYear = yyyy + 1,
            dayMonth = "09/30/",
            birthday = dayMonth + yyyy;

        today = mm + "/" + dd + "/" + yyyy;
        if (today > birthday) {
          birthday = dayMonth + nextYear;
        }
        //end

        const countDown = new Date(birthday).getTime(),
            x = setInterval(function() {    

              const now = new Date().getTime(),
                    distance = countDown - now;

              document.getElementById("days").innerText = Math.floor(distance / (day)),
                document.getElementById("hours").innerText = Math.floor((distance % (day)) / (hour)),
                document.getElementById("minutes").innerText = Math.floor((distance % (hour)) / (minute)),
                document.getElementById("seconds").innerText = Math.floor((distance % (minute)) / second);

              //do something later when date is reached
              if (distance < 0) {
                document.getElementById("countdown").style.display = "none";
                document.getElementById("content").style.display = "block";
                clearInterval(x);
              }
              //seconds
            }, 0);

    };

    return {
      sendNotification,
      isInCart,
      addItemToCart,
      productGetters,
      productsLoading,
      products: computed(() =>
        productGetters.getFeaturedProductsFiltered(featureProducts.value)
      ),
      toggleWishlist,
      toggleNewsletterModal,
      onSubscribe,
      addBasePath,
      banners,
      heroes,
      timer
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
  middleware: cacheControl({
    'max-age': 60,
    'stale-when-revalidate': 5
  }),
  components: {
    InstagramFeed,
    SfHero,
    SfBanner,
    SfCallToAction,
    SfSection,
    SfCarousel,
    SfProductCard,
    SfImage,
    SfBannerGrid,
    SfHeading,
    SfArrow,
    SfButton,
    NewsletterModal,
    OffersCounter,
    LazyHydrate
  }
};
</script>

<style lang="scss" scoped>
#home {
  box-sizing: border-box;
  padding: 0 var(--spacer-sm);
  @include for-desktop {
    max-width: 1240px;
    padding: 0;
    margin: 0 auto;
  }
}

.hero {
  margin: 0;
  border-bottom: 10px solid #0caa91;
  --hero-item-background-position: center;
  @include for-desktop {
    margin: 0;
  }
  .sf-hero-item {
    &:nth-child(even) {
      --hero-item-background-position: left;
      @include for-mobile {
        --hero-item-background-position: 30%;
        ::v-deep .sf-hero-item__subtitle,
        ::v-deep .sf-hero-item__title {
          text-align: right;
          width: 100%;
          padding-left: var(--spacer-sm);
        }
      }
    }
  }
  ::v-deep .sf-hero__control {
    &--right, &--left {
      display: none;
    }
  }
}

.banner-grid {
  --banner-container-width: 50%;
  margin: var(--spacer-xl) 0;
  ::v-deep .sf-link:hover {
    color: var(--c-white);
  }
  @include for-desktop {
    margin: var(--spacer-2xl) 0;
    ::v-deep .sf-link {
      --button-width: auto;
      text-decoration: none;
    }
  }
}

.banner {
  &__tshirt {
    background-position: left;
  }
  &-central {
    @include for-desktop {
      --banner-container-flex: 0 0 70%;
    }
  }
}

.similar-products {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: var(--spacer-2xs);
  --heading-padding: 0;
  border-bottom: 1px var(--c-light) solid;
  @include for-desktop {
    border-bottom: 0;
    justify-content: center;
    padding-bottom: 0;
  }
}

.call-to-action {
  background-position: right;
  margin: var(--spacer-xs) 0;
  @include for-desktop {
    margin: var(--spacer-xl) 0 var(--spacer-2xl) 0;
  }
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

</style>

<style lang="scss">
.sf-circle-icon{
  z-index: 10;
  bottom: var(--product-card-add-button-bottom, 0rem);
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

#home .carousel-cats .glide__track{
    background: #f7f7f7;
    border-radius: 15px;
}

.categories .category-item{
}

#home .carousel{
  margin-top: 2rem;
}

.categories{
    display: flex;
    flex: 30%;
    flex-wrap: wrap;
    margin-top: 5rem;
    margin-bottom: 5rem;
}

.categories .category-item{
  width: calc(100% / 3);
}

.category-item {
  margin-bottom: 1rem;
}
.categories.categories .category-item a{
  position: relative;
  display: flex;
}

.info-section .category-item a{
  flex-direction: column;
}

.category-item img{
    height: 220px !important;
    width: 95% !important;
    display: flex;
}

.category-item span{
  position: absolute;
    height: 100%;
    width: 95%;
    top: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #fff;
    font-size: 40px;
    font-weight: bold;
    background: #0000001a;
}

.sf-arrow--long, .sf-arrow{
background-color: #2F9393;
}

.info-section{
  background: #F7F7F7;
  height: 30vh;
  
}

.carousel-counter{
  margin: 2rem 0rem !important; 
  background: rgb(46,142,142);
  background: linear-gradient(56deg, rgba(46,142,142,1) 0%, rgba(47,211,147,1) 100%);
  border-radius: 0px 15px 15px 0px;
  display: flex;
  width: 100%;
  height: 50vh;
}

.carousel-counter .carousel-counter-col{
  width: 50%;
  display: flex;
  align-items: center;
}

.carousel-counter .carousel-counter-col .sf-carousel__wrapper{
  height: 100%;
  max-height: 100%;
  align-items: center;
  display: block;
  max-width: 95%;
}

.carousel-counter .carousel-counter-col .carousel{
  background: #fff;
  height: 85%;
  width: 95%;
  border-radius: 0px 15px 15px 0px;
  margin: 0 !important;
}

.carousel-counter .sf-carousel__controls{
  display: none;
}

.carousel-counter .carousel-item-counter{
  background: #fff;
  display: flex;
}

.carousel-counter .carousel-item-counter .carousel-item-counter-image{
  padding-right: 1rem;
    border-right: 1px solid #2f9393;
    align-items: center;
    display: flex;

}

.carousel-counter .carousel-item-counter .carousel-item-counter-image img{
  width: 85%;
}

.carousel-counter .carousel-item-counter .carousel-item-discount{
  background: #2f9393;
    color: #fff;
    width: fit-content;
    padding: 2px 4px;
}

.carousel-counter .carousel-item-counter .carousel-item-brand{
    padding: 5px 0;
    font-size: 22px;
    font-weight: bold;
    text-align: start;
}

.carousel-counter .carousel-item-counter .carousel-item-name{
    text-align: start;
    color: #808080;
    font-size: 25px;
    padding-bottom: 5px;
}

.carousel-counter .carousel-item-counter .carousel-item-specialprice{
    text-align: start;
    font-size: 35px;
    font-weight: bolder;
    color: #2F9393;
    letter-spacing: 0.5px;
}

.carousel-counter .carousel-item-counter .carousel-item-normalprice{
    text-align: start;
    font-size: 14px;
    color: #a7a7a7;
}

.carousel-counter .carousel-item-counter .carousel-item-button{
    text-align: start;
    background: #2F9393;
    color: #fff;
    padding: 8px 14px !important;
    font-size: 20px;
    font-weight: bold;
    position: relative;
    display: block;
    margin-top: 10px;
    width: fit-content;
}

.carousel-counter .carousel-counter-col.counter{
  flex-direction: column;
  text-align: center;
  justify-content: center;
}

.carousel-counter .carousel-counter-col.counter .carousel-counter-title{
    color: #fff;
    font-size: 16px;
    padding-bottom: 1rem;
    margin-bottom: 1rem;
}

#countdown li {
  display: inline-block;
  font-size: 1.5em;
  list-style-type: none;
  padding: 1em;
  text-transform: uppercase;
  font-size: 10px;
  color: #fff;
}

#countdown ul{
  display: flex;
  align-items: center;
  padding: 0;
}

#countdown li span {
  display: block;
  font-size: 2.5rem;
  color: #2f9393;
  background: #fff;
  padding: 10px;
  border-radius: 10px;
  width: 50px;
  margin: 0 auto;
  margin-bottom: 5px;

}

#countdown li span::first-letter{
    margin: 2px;
}

#countdown .timer-separator{
  display: inline-block;
  color: #fff;
  font-weight: bold;
}

.sf-heading__title.h2{
  margin-top: 2rem;
}

.carousel-counter hr{
    width: 65%;
    color: #ffffff8c;
    height: 10px;
    border-top: none;
    border-right: none;
    border-left: none;
}

.carousel-counter .carousel-counter-logo{
  text-align: start;
  width: 65%;
    
}

.carousel-counter .carousel-counter-logo img{
    width: 30%;
    text-align: start;
}

</style>