<template>

  <div>
      <div class="info-section">
        <div class="categories">

            <div class="category-item">
              <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/icons/SVG/venta.svg" srcset="" sizes=""  class="sf-image-loaded" style="height: 128px !important; width: auto">
              <br>
              <b>Venta telefónica</b>
              <br>
              Si deseas ayuda con tu compra
              </nuxt-link>
            </div>

            <div class="category-item">
              <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/icons/SVG/despacho.svg" srcset="" sizes=""  class="sf-image-loaded" style="height: 128px !important; width: auto">
                <br>
              <b>Despacho</b>
              <br>
              A todo Chile
              
              </nuxt-link>
            </div>

            <div class="category-item">
              <nuxt-link :to="localePath({ name: 'home' })" class="bold"><img loading="lazy" src="/icons/SVG/retiro.svg" srcset="" sizes=""  class="sf-image-loaded" style="height: 128px !important; width: auto">
              
                <br>
              <b>Retiro en tienda</b>
              <br>
              Gratis, si no deseas pagar envío
              </nuxt-link>
            </div>

          </div>
      </div>
  <SfFooter :column="4" multiple class="footer">
    <SfFooterColumn title="Más información">
      <SfList>
        <SfListItem
          v-for="item in aboutUs"
          :key="item"
          >
          <SfMenuItem
            :label="$t(item)"
          />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn title="Contacto">
      <SfList>
        <SfListItem>
        <SfMenuItem
            label="DLDS distribuidores"
          />
          <br>
          <SfMenuItem
            label="xequiel Fernández 3685, Macul
      Retiro de productos Bodega 281,
      Reuniones Oficina 231"
          />
          <br>
          <hr>
          <br>
          <SfMenuItem
            label="ventas@dlds.cl"
          />
        
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn title="Horarios">
      <SfList>
        <SfListItem>
          <SfMenuItem
            label="Horarios de atención al cliente"
          />
          <SfMenuItem
            label="Lunes a Jueves: 08:30 a 18:00 Horas."
          />
          <SfMenuItem
            label="Viernes:  08:30 a 16:45 Horas"
          />
          <br><br>
          <SfMenuItem
            label="Ventas: +569 8158 1571"
          />
          <SfMenuItem
            label="Postventa: +56 9 4627 3761"
          />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn title="" style="padding: 1rem;">
      <SfImage class="footer__social-image"  :src="addBasePath('/icons/logo.png')" height="44px" alt="Logo"/>
      <br>
      <div class="footer__socials">
        <SfImage class="footer__social-image" v-for="item in social" :key="item" :src="addBasePath('/icons/'+item+'.svg')" :alt="item" width="32" height="32" />
      </div>
    </SfFooterColumn>
    <SfFooterColumn title="Medios de pago">
      <div class="footer__socials pagos">
        <SfImage class="footer__social-image" :src="addBasePath('/icons/medios-de-pago.png')" height="52" style="width: 100%;" alt="Medios de pago"/>
      </div>
    </SfFooterColumn>
  </SfFooter>
  </div>

</template>

<script>
import { SfFooter, SfList, SfImage, SfMenuItem } from '@storefront-ui/vue';
import { addBasePath } from '@vue-storefront/core';
import LazyHydrate from 'vue-lazy-hydration';

export default {
  components: {
    SfFooter,
    SfList,
    SfImage,
    SfMenuItem
  },
  setup() {
    return {
      addBasePath
    };
  },
  data() {
    return {
      aboutUs: ['> Ofertas', '> Nodevades', '> Lo más vendido', '> Mi cuenta', '> Envíos y garantías', '> Nosotros', '> Aviso de privacidad'],
      departments: ['Women fashion', 'Men fashion', 'Kidswear', 'Home'],
      help: ['Customer service', 'Size guide', 'Contact us'],
      paymentsDelivery: ['Purchase terms', 'Guarantee'],
      social: ['instagram', 'facebook'],
      isMobile: false,
      desktopMin: 1024
    };
  }
};
</script>

<style lang="scss">

.footer {
  margin-bottom: 3.75rem;
  @include for-desktop {
    margin-bottom: 0;
  }
  &__socials {
    display: flex;
    justify-content: space-between;
    margin: 0 auto var(--spacer-lg);
    margin-top: 20px;
    padding: var(--spacer-base) var(--spacer-xl);
    @include for-desktop {
      justify-content: flex-end;
      padding: var(--spacer-xs) 0;
      margin: 0 auto;
    }
  }
  &__social-image {
    margin: 0 var(--spacer-2xs) 0 0;
  }
}

.footer .footer__socials .footer__social-image img{
  width: 40px;
}

.footer .footer__socials.pagos .footer__social-image img{
  width: 100% !important;
}

.sf-footer {
  @include for-desktop {
    border-top: 15px solid #0CAA91;
    padding-bottom: 0;
    margin-top: 0;
    background-color: #2F9393;
    padding-right: 2rem;
    padding-left: 2rem;
  }
  &__container {
    margin: var(--spacer-sm);
    @include for-desktop {
      max-width: 100%;
      margin: 0 auto;
    }
  }
}

.sf-footer  .sf-footer-column{
  padding: 1rem 0rem 1rem 4rem;
}

.sf-footer-column, .sf-footer-column__title{
  background-color: #2F9393;
}

.info-section{
  margin-top: 5rem;
  padding-bottom: 5rem !important;
}

.info-section .categories{
    display: flex;
    flex: 30%;
    flex-wrap: wrap;
    margin-top: 5rem;
    margin-bottom: 5rem;
    width: 70%;
    margin-right: auto;
    margin-left: auto;
}

.categories .category-item{
  width: calc(100% / 3);
}

.info-section .categories .category-item:nth-child(2){
    border-right: 1px solid #000;
    border-left: 1px solid #000;
    width: 33%;
}

.category-item {
  margin-bottom: 1rem;
  text-align: center;
}
.category-item a{
  position: relative;
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

.sf-arrow--long{
background-color: #2F9393;
}

.info-section{
  background: #F7F7F7;
  height: 30vh; 
  padding: 2rem;
  
}
</style>
