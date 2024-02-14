<template>
  <div id="nosotros">
    <SfBreadcrumbs
      class="breadcrumbs desktop-only"
      :breadcrumbs="breadcrumbs"
    />

    <div class="page-title">
      <h2>Sobre nosotros</h2>
    </div>

    <div class="page-content">

      <div class="page-content-image">
        <img src="/img/nosotros.png" alt="">
      </div>

      <div class="page-content-text">
        <div class="page-content-item">
          <h3>Misión</h3>
          <p>Somos la distribuidora y comercializadora mayorista que ofrece el catálogo más amplio de insumos para la industria cannábica, gracias a una colaboración estrecha con nuestros proveedores y al profesionalismo de nuestros colaboradores, lo que nos permite entregar un servicio óptimo a dispensarios, tiendas de cultivo, universidades, centros de estudios y fundaciones a lo largo de todo Chile.</p>
        </div>
        <div class="page-content-item">
          <h3>Visión</h3>
          <p>Somos la distribuidora y comercializadora mayorista que ofrece el catálogo más amplio de insumos para la industria cannábica, gracias a una colaboración estrecha con nuestros proveedores y al profesionalismo de nuestros colaboradores, lo que nos permite entregar un servicio óptimo a dispensarios, tiendas de cultivo, universidades, centros de estudios y fundaciones a lo largo de todo Chile.</p>
        </div>
        <div class="page-content-item">
          <h3>Valores</h3>
          <p>Somos la distribuidora y comercializadora mayorista que ofrece el catálogo más amplio de insumos para la industria cannábica, gracias a una colaboración estrecha con nuestros proveedores y al profesionalismo de nuestros colaboradores, lo que nos permite entregar un servicio óptimo a dispensarios, tiendas de cultivo, universidades, centros de estudios y fundaciones a lo largo de todo Chile.</p>
        </div>
      </div>
    </div>
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
</template>
<script>
import { SfBreadcrumbs, SfContentPages, SfCarousel, SfArrow, SfProductCard, SfHeading } from '@storefront-ui/vue';
import { computed, onBeforeUnmount, useRoute, useRouter } from '@nuxtjs/composition-api';
import { useUser } from '@vue-storefront/prestashop';
import MyProfile from './MyAccount/MyProfile';
import Addresses from './MyAccount/Addresses';
import BillingDetails from './MyAccount/BillingDetails';
import MyNewsletter from './MyAccount/MyNewsletter';
import OrderHistory from './MyAccount/OrderHistory';
import LazyHydrate from 'vue-lazy-hydration';
import { useUiNotification } from '~/composables';
import {
  mapMobileObserver,
  unMapMobileObserver
} from '@storefront-ui/vue/src/utilities/mobile-observer.js';

import {
  useProduct,
  productGetters,
  useCart
} from '@vue-storefront/prestashop';

export default {
  name: 'Nosotros',
  components: {
    SfBreadcrumbs,
    SfContentPages,
    MyProfile,
    Addresses,
    BillingDetails,
    MyNewsletter,
    OrderHistory,
    SfCarousel,
    SfArrow,
    SfProductCard,
    LazyHydrate,
    SfHeading
  },
  middleware: [
    'is-authenticated'
  ],
  setup(props, context) {
    const route = useRoute();
    const router = useRouter();

    const { logout } = useUser();
    const isMobile = computed(() => mapMobileObserver().isMobile.get());
    const activePage = computed(() => {
    const { pageName } = route.value.params;
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

      if (pageName) {
        return (pageName.charAt(0).toUpperCase() + pageName.slice(1)).replace('-', ' ');
      } else if (!isMobile.value) {
        return 'My profile';
      } else {
        return '';
      }
    });

    const changeActivePage = async (title) => {
      if (title === 'Log out') {
        await logout();
        router.push(context.root.localePath({ name: 'home' }));
        return;
      }

      const slugifiedTitle = (title || '').toLowerCase().replace(' ', '-');
      const transformedPath = `/my-account/${slugifiedTitle}`;
      const localeTransformedPath = context.root.localePath(transformedPath);

      router.push(localeTransformedPath);
    };

    onBeforeUnmount(() => {
      unMapMobileObserver();
    });

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

    return {       
      sendNotification,
      isInCart,
      addItemToCart,
      changeActivePage,
      productGetters,
      productsLoading,
      products: computed(() =>
        productGetters.getFeaturedProductsFiltered(featureProducts.value)
      ), 
      toggleWishlist,
      activePage};
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
  data() {
    return {
      breadcrumbs: [
        {
          text: 'Inicio',
          route: { link: '#' }
        },
        {
          text: 'Sobre nosotros',
          route: { link: '#' }
        }
      ]
    };
  }
};
</script>

<style lang='scss' scoped>
#nosotros {
  box-sizing: border-box;
  @include for-desktop {
    max-width: 1240px;
    margin: 0 auto;
  }
}
.my-account {
  @include for-mobile {
    --content-pages-sidebar-category-title-font-weight: var(
      --font-weight--normal
    );
    --content-pages-sidebar-category-title-margin: var(--spacer-sm)
      var(--spacer-sm) var(--spacer-sm) var(--spacer-base);
  }
  @include for-desktop {
    --content-pages-sidebar-category-title-margin: var(--spacer-xl) 0 0 0;
  }
}

.similar-products{
  margin-top: 2rem;
}
.breadcrumbs {
  margin: var(--spacer-base) 0 var(--spacer-lg);
}

.page-title{
  background-color: #0CAA91;
  padding: 5px;
  width: 100%;
  margin-bottom: 20px;
}

.page-title h2{
  font-size: 15px;
  color: #fff;
}

.page-content{
  display: flex;
}

.page-content .page-content-image{
  width: 50%;
  padding: 10px;
}
.page-content .page-content-image img{
  width: 100%;
  
}
.page-content .page-content-text{
  width: 50%;
  padding: 0 10px;
}

.page-content .page-content-text .page-content-item{
  margin-bottom: 10px;
}

.page-content .page-content-text .page-content-item h3{
  color: #0CAA91;
  text-transform: capitalize;
  font-size: 15px;
}

.page-content .page-content-text .page-content-item p{
  color: #000;
  font-size: 13px;
  font-weight: 500;
  text-align: justify;
}

.carousel__item{
  text-align: center;
  justify-content: center;

}


.carousel__item .carousel__item__product{
  border: 2px solid #eee;

}
.carousel__item img{
  margin-bottom: 10px;
}

#nosotros .carousel-cats .glide__track{
    background: #f7f7f7;
    border-radius: 15px;
}

#nosotros .carousel{
  margin-top: 2rem;
}

.sf-arrow--long, .sf-arrow{
background-color: #2F9393;
}

@media(max-width: 720px){

  .page-content{
    flex-direction: column;
  }

  .page-content .page-content-image{
    width: 100%;
    padding: 10px;
    margin-bottom: 1rem;
    max-width: fit-content;
  }
  .page-content .page-content-image img{
    width: 100%;
    
  }
  .page-content .page-content-text{
    width: 100%;
    padding: 10px;
    max-width: fit-content;
  }

}

</style>
