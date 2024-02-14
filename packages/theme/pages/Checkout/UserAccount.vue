<template>
  
  <ValidationObserver v-slot="{ handleSubmit }">


      <div v-if="!isAuthenticated">
        <div class="user-div">

            <SfHeading
            v-e2e="'user-account-heading'"
            :level="3"
            title="Datos personales"
            class="sf-heading--left sf-heading--no-underline title"
          />
            <div class="mb-20">
              Register |
              <SfButton class="sf-button--text" @click="handleAccountClick">
                {{ $t('Sign in') }}
              </SfButton>
            </div>

            <CreateAccountForm @submit="updatePersonalData"/>
          </div>
    </div>
    <div v-if="isAuthenticated">
      <div class="user-div">
          <SfHeading
          v-e2e="'user-account-heading'"
          :level="3"
          title="Datos personales"
          class="sf-heading--left sf-heading--no-underline title"
        />
          <div class="checkout-user-details">
            <span class="user-email">{{ form.email }} <span class="no-logged">No soy yo. <SfLink @click="logOut" >Cerrar sesión</SfLink></span></span> 
            <span class="user-name">Nombre: {{ form.firstName }} {{ form.lastName }}</span>
            <span class="user-mobile-phone">Celular: +569 1234 5678</span>
          </div>
          <div class="checkout-user-actions">
            <SfLink><img src="/icons/invoice.svg"/>¿Necesitas facura? Ingresa los datos para emitirla</SfLink>
            <SfLink><img src="/icons/edit.svg"/>editar datos</SfLink>
          </div>
        </div>

        <div class="user-div entrega">
          <SfHeading
          v-e2e="'user-account-heading'"
          :level="3"
          title="Información de entrega"
          class="sf-heading--left sf-heading--no-underline title"
        />
          <div class="checkout-user-details address" v-for="address in addressesList" :key="address.id">
                  <span class="user-email">Región: {{address.city}}</span> 
                  <span class="user-name">Dirección: {{address.alias}}</span>
                  <span class="user-mobile-phone">Tipo de entrega: Despacho o domicilio</span>
          </div>
          <div class="checkout-user-actions">
            <SfLink><img src="/icons/edit.svg"/>editar entrega</SfLink>
          </div>
        </div>
    </div>
  </ValidationObserver>
</template>

<script>
import {
  SfHeading,
  SfInput,
  SfButton,
  SfSelect,
  SfLink
} from '@storefront-ui/vue';

import { computed, ref } from '@nuxtjs/composition-api';
import { useUiState } from '~/composables';
import { onSSR } from '@vue-storefront/core';
import {useShipping, useUser, userGetters, userShippingGetters, useUserShipping} from '@vue-storefront/prestashop';
import {required, min, email, confirmed} from 'vee-validate/dist/rules';
import { ValidationProvider, ValidationObserver, extend } from 'vee-validate';
import CreateAccountForm from '~/components/MyAccount/CreateAccountForm';


const COUNTRIES = [
  { key: 'US', label: 'United States' },
  { key: 'UK', label: 'United Kingdom' },
  { key: 'IT', label: 'Italy' },
  { key: 'PL', label: 'Poland' }
];

extend('email', {
  ...email,
  message: 'Invalid email'
});

extend('required', {
  ...required,
  message: 'This field is required'
});

extend('min', {
  ...min,
  message: 'The field should have at least {length} characters'
});

extend('password', {
  validate: value => String(value).length >= 8,
  message: 'Password must have at least 8 characters'
});

extend('confirmed', {
  ...confirmed,
  message: 'Passwords don\'t match'
});

extend('nothavenumber', {
  validate: value => String(value).match(/^([^0-9]*)$/),
  message: 'Bad format - Cannot contain a number'
});

export default {
  name: 'Shipping',
  components: {
    SfHeading,
    SfInput,
    SfButton,
    SfSelect,
    SfLink,
    ValidationProvider,
    ValidationObserver,
    CreateAccountForm,
    VsfShippingProvider: () => import('~/components/Checkout/VsfShippingProvider')
  },
  setup (props, context) {
    const { $router } = context.root;
    const isFormSubmitted = ref(false);
    const { load, save, loading } = useShipping();
    const { toggleLoginModal } = useUiState();
    const { isAuthenticated, logout, register } = useUser();
    const { shipping } = useUserShipping();
    const addressesList = computed(()=> shipping.value ? userShippingGetters.getAddresses(shipping.value) : []);
    const { user } = useUser();

    const resetForm = () => ({
      firstName: userGetters.getFirstName(user.value),
      lastName: userGetters.getLastName(user.value),
      email: userGetters.getEmailAddress(user.value),
      gender: userGetters.getGender(user.value)
    });

    // const form = ref({
    //   firstName: '',
    //   lastName: '',
    //   streetName: '',
    //   apartment: '',
    //   city: '',
    //   state: '',
    //   country: '',
    //   postalCode: '',
    //   phone: null
    // });

    const form = ref(resetForm());

    const handleFormSubmit = async () => {
      await save({ shippingDetails: form.value });
      isFormSubmitted.value = true;
    };

    onSSR(async () => {
      await load();
    });

    const handleAccountClick = async () => {
      toggleLoginModal();
    };

    const logOut = async () => {
      await logout();
      $router.push(context.root.localePath({ name: 'home' }));
    };

    const formHandler = async (fn, onComplete, onError) => {
      try {
        const data = await fn();
        await onComplete(data);
      } catch (error) {
        onError(error);
      }
    };

    const updatePersonalData = ({ form, onComplete, onError }) => formHandler(() => register({ user: form.value }), onComplete, onError);

    return {
      isAuthenticated,
      logOut,
      handleAccountClick,
      loading,
      isFormSubmitted,
      form,
      countries: COUNTRIES,
      handleFormSubmit,
      updatePersonalData,
      addressesList,
      shipping
    };
  }
};
</script>

<style lang="scss" scoped>
.form {
  --button-width: 100%;
  &__select {
    display: flex;
    align-items: center;
    --select-option-font-size: var(--font-size--lg);
    ::v-deep .sf-select__dropdown {
      font-size: var(--font-size--lg);
      margin: 0;
      color: var(--c-text);
      font-family: var(--font-family--secondary);
      font-weight: var(--font-weight--normal);
    }
  }
  @include for-desktop {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    --button-width: auto;
  }
  &__element {
    margin: 0 0 var(--spacer-xl) 0;
    @include for-desktop {
      flex: 0 0 100%;
    }
    &--half {
      @include for-desktop {
        flex: 1 1 50%;
      }
      &-even {
        @include for-desktop {
          padding: 0 0 0 var(--spacer-xl);
        }
      }
    }
  }
  &__action {
    @include for-desktop {
      flex: 0 0 100%;
      display: flex;
    }
  }
  &__action-button {
    &--secondary {
      @include for-desktop {
        order: -1;
        text-align: left;
      }
    }
    &--add-address {
      width: 100%;
      margin: 0;
      @include for-desktop {
        margin: 0 0 var(--spacer-lg) 0;
        width: auto;
      }
    }
  }
  &__back-button {
    margin: var(--spacer-xl) 0 var(--spacer-sm);
    &:hover {
      color:  var(--c-white);
    }
    @include for-desktop {
      margin: 0 var(--spacer-xl) 0 0;
    }
  }
}

.shipping {
  &__label {
    display: flex;
    justify-content: space-between;
  }
  &__description {
    --radio-description-margin: 0;
    --radio-description-font-size: var(--font-xs);
  }
}

.title {
  margin: var(--spacer-xl) 0 var(--spacer-base) 0;
}

.mb-20{
  margin-bottom: 20px;
}

.checkout-user-details{
  display: flex;
  flex-direction: column;
}

.checkout-user-details .user-email, .checkout-user-details .user-name, .checkout-user-details .user-mobile-phone{
  padding-bottom: 1rem;
}

.user-div{
  padding-bottom: 3rem;
  border-bottom: 1px solid #009688;
}

.checkout-user-details .no-logged{
  color: #0caa91;
  margin-left: 10px;

  a{
    color: #0caa91;
    text-decoration: none;
  }
  

}

.checkout-user-details.address{
    margin-bottom: 1rem;
  }

.entrega .checkout-user-actions{
  justify-content: end;
}
.checkout-user-actions{
  display: flex;
  justify-content: end;

  a:first-child{
    padding-right: 1rem;
  }

  a{
    font-size: 16px;
    align-items: center;
    display: flex;
    font-weight: bold;
    img{
      width: 20px;
      margin-right: 5px;
    }
  }
}
</style>
