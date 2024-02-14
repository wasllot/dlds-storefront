<template>

  <div id="registration">
    <SfBreadcrumbs
        class="breadcrumbs desktop-only"
        :breadcrumbs="breadcrumbs"
      />

    <div class="page-title">
      <h2>Crear una cuenta</h2>
    </div>

    <div class="page-content">

      <div class="page-content-text">
        <transition name="sf-fade" mode="out-in">
      <div v-if="currentScreen === SCREEN_LOGIN">
        <ValidationObserver v-slot="{ handleSubmit }" key="log-in">
          <span class="login-title">Inicia sesión con tu cuenta</span>
          <form class="form" @submit.prevent="handleSubmit(handleLogin)">
            
            <ValidationProvider rules="required|email" v-slot="{ errors }">
              <span>Correo Electrónico</span>
              <SfInput
                v-e2e="'login-modal-email'"
                v-model="form.username"
                :valid="!errors[0]"
                :errorMessage="errors[0]"
                name="email"
                label="ejemplo: correo@gmail.com"
                class="form__element"
              />
            </ValidationProvider>
            <ValidationProvider rules="required" v-slot="{ errors }">
              <span>Contraseña</span>
              <SfInput
                v-e2e="'login-modal-password'"
                v-model="form.password"
                :valid="!errors[0]"
                :errorMessage="errors[0]"
                name="password"
                label="*********"
                type="password"
                class="form__element form-password"
              />
              <div class="action">
                <SfButton class="sf-button--text" @click="setCurrentScreen(SCREEN_FORGOTTEN)">
                  ¿Olvidó su contraseña?
                </SfButton>
              </div>
            </ValidationProvider>
            <div v-if="error.login">
              {{ error.login }}
            </div>
            <SfButton v-e2e="'login-modal-submit'"
              type="submit"
              class="sf-button--full-width form__button"
              :disabled="loading"
            >
              <SfLoader :class="{ loader: loading }" :loading="loading">
                <div>INGRESAR A MI CUENTA</div>
              </SfLoader>
            </SfButton>
          </form>
        </ValidationObserver>

        <div class="bottom">
          <p class="bottom__paragraph">¿No estás registrado?</p>
          <nuxt-link class="sf-button--text" :to="localePath({ name: 'registration' })">Crea una cuenta aquí</nuxt-link>
        </div>
      </div>
      <div v-else-if="currentScreen === SCREEN_FORGOTTEN">
        <p>{{ $t('Forgot Password') }}</p>
        <ValidationObserver v-slot="{ handleSubmit }" key="log-in">
          <form class="form" @submit.prevent="handleSubmit(handleForgotten)">
            <ValidationProvider rules="required|email" v-slot="{ errors }">
              <SfInput
                v-e2e="'forgot-modal-email'"
                v-model="form.username"
                :valid="!errors[0]"
                :errorMessage="errors[0]"
                name="email"
                :label="$t('Forgot Password Modal Email')"
                class="form__element"
              />
            </ValidationProvider>
            <div v-if="forgotPasswordError.request">
              {{ forgotPasswordError.request.message }}
            </div>
            <SfButton
              v-e2e="'forgot-modal-submit'"
              type="submit"
              class="sf-button--full-width form__button"
              :disabled="forgotPasswordLoading"
            >
              <SfLoader :class="{ loader: forgotPasswordLoading }" :loading="forgotPasswordLoading">
                <div>{{ $t('Reset Password') }}</div>
              </SfLoader>
            </SfButton>
          </form>
        </ValidationObserver>
      </div>
      <div v-else-if="currentScreen === SCREEN_THANK_YOU" class="thank-you">
        <i18n tag="p" class="thank-you__paragraph" path="forgotPasswordConfirmation">
          <span class="thank-you__paragraph--bold">{{ userEmail }}</span>
        </i18n>
        <p class="thank-you__paragraph">{{ $t('Thank You Inbox') }}</p>
      </div>
      <div v-else class="form">
        <div class="bottom">
          <p class="bottom__paragraph">¿Estás registrado?</p>
          <nuxt-link class="sf-button--text" :to="localePath({ name: 'login' })" >Inicia sesión aquí</nuxt-link>
        </div>
        <ValidationObserver v-slot="{ handleSubmit }" key="sign-up">
          <form class="form" @submit.prevent="handleSubmit(handleRegister)" autocomplete="off">
            <ValidationProvider rules="required" v-slot="{ errors }">
              <label for="first-name">Nombre</label>
              <SfInput
                v-e2e="'login-modal-firstName'"
                v-model="form.firstName"
                :valid="!errors[0]"
                :errorMessage="errors[0]"
                name="first-name"
                class="form__element"
              />
            </ValidationProvider>
            <ValidationProvider rules="required" v-slot="{ errors }">
              <label for="last-name">Apellido</label>
              <SfInput
                v-e2e="'login-modal-lastName'"
                v-model="form.lastName"
                :valid="!errors[0]"
                :errorMessage="errors[0]"
                name="last-name"
                class="form__element"
              />
            </ValidationProvider>
            <ValidationProvider rules="required|email" v-slot="{ errors }">
              <label for="email">Correo</label>

              <SfInput
                v-e2e="'login-modal-email'"
                v-model="form.email"
                :valid="!errors[0]"
                :errorMessage="errors[0]"
                name="email"
                class="form__element"
              />
            </ValidationProvider>
            <ValidationProvider rules="required" v-slot="{ errors }">
              <label for="password">Contraseña</label>
              <SfInput
                v-e2e="'login-modal-password'"
                v-model="form.password"
                :valid="!errors[0]"
                :errorMessage="errors[0]"
                name="password"
                type="password"
                class="form__element password"
              />
            </ValidationProvider>
            <!-- <ValidationProvider :rules="{ required: { allowFalse: false } }" v-slot="{ errors }">
              <SfCheckbox
                v-e2e="'login-modal-create-account'"
                v-model="createAccount"
                :valid="!errors[0]"
                :errorMessage="errors[0]"
                name="create-account"
                :label="$t('I want to create an account')"
                class="form__element"
              />
            </ValidationProvider> -->
            <div v-if="error.register">
              {{ error.register }}
            </div>
            <SfButton
              type="submit"
              class="sf-button--full-width form__button"
              :disabled="loading"
            >
              <SfLoader :class="{ loader: loading }" :loading="loading">
                <div>CREAR CUENTA</div>
              </SfLoader>
            </SfButton>
          </form>
        </ValidationObserver>
      </div>
    </transition>
      </div>
    </div>


  </div>



</template>
<script>
import { ref, watch, reactive, computed } from '@nuxtjs/composition-api';
import { SfModal, SfInput, SfButton, SfCheckbox, SfLoader, SfAlert, SfBar, SfBreadcrumbs } from '@storefront-ui/vue';
import { ValidationProvider, ValidationObserver, extend } from 'vee-validate';
import { required, email } from 'vee-validate/dist/rules';
import { useUser, useForgotPassword } from '@vue-storefront/prestashop';
import { useUiState } from '~/composables';

extend('email', {
  ...email,
  message: 'Invalid email'
});

extend('required', {
  ...required,
  message: 'This field is required'
});

export default {
  name: 'Register',
  components: {
    SfModal,
    SfInput,
    SfButton,
    SfCheckbox,
    SfLoader,
    SfAlert,
    ValidationProvider,
    ValidationObserver,
    SfBar,
    SfBreadcrumbs
  },
  setup() {
    const SCREEN_LOGIN = 'login';
    const SCREEN_REGISTER = 'register';
    const SCREEN_THANK_YOU = 'thankYouAfterForgotten';
    const SCREEN_FORGOTTEN = 'forgottenPassword';

    const { isLoginModalOpen, toggleLoginModal } = useUiState();
    const form = ref({});
    const userEmail = ref('');
    const createAccount = ref(false);
    const rememberMe = ref(false);
    const { register, login, loading, error: userError } = useUser();
    const { request, error: forgotPasswordError, loading: forgotPasswordLoading } = useForgotPassword();
    const currentScreen = ref(SCREEN_REGISTER);

    const error = reactive({
      login: null,
      register: null
    });

    const resetErrorValues = () => {
      error.login = null;
      error.register = null;
    };

    const barTitle = computed(() => {
      switch (currentScreen.value) {
        case SCREEN_LOGIN:
          return 'Sign in';
        case SCREEN_REGISTER:
          return 'Register';
        default:
          return 'Reset Password';
      }
    });

    watch(isLoginModalOpen, () => {
      if (isLoginModalOpen) {
        form.value = {};
        resetErrorValues();
      }
    });

    const setCurrentScreen = (screenName) => {
      resetErrorValues();

      if(screenName != 'SCREEN_REGISTER'){
        currentScreen.value = screenName;
      }else{

      }

    };

    const handleForm = (fn) => async () => {
      resetErrorValues();
      await fn({ user: form.value });

      const hasUserErrors = userError.value.register || userError.value.login;

      if (hasUserErrors) {
        error.login = userError.value.login?.message;
        error.register = userError.value.register?.message;
        return;
      }
      toggleLoginModal();
    };

    const closeModal = () => {
      resetErrorValues();
      setCurrentScreen(SCREEN_LOGIN);
      toggleLoginModal();
    };

    const handleRegister = async () => handleForm(register)();

    const handleLogin = async () => handleForm(login)();

    const handleForgotten = async () => {
      userEmail.value = form.value.username;
      await request({ email: userEmail.value });

      if (!forgotPasswordError.value.request) {
        setCurrentScreen(SCREEN_THANK_YOU);
      }
    };

    return {
      form,
      error,
      userError,
      loading,
      createAccount,
      rememberMe,
      isLoginModalOpen,
      toggleLoginModal,
      handleLogin,
      handleRegister,
      forgotPasswordError,
      forgotPasswordLoading,
      handleForgotten,
      closeModal,
      userEmail,
      barTitle,
      currentScreen,
      setCurrentScreen,
      SCREEN_LOGIN,
      SCREEN_REGISTER,
      SCREEN_THANK_YOU,
      SCREEN_FORGOTTEN
    };
  },
  data() {
    return {
      breadcrumbs: [
        {
          text: 'Inicio',
          route: { link: '#' }
        },
        {
          text: 'Registro',
          route: { link: '#' }
        }
      ]
    };
  }
};
</script>

<style lang="scss" scoped>

.modal {
  --modal-index: 3;
  --overlay-z-index: 3;
}
.form {
  margin-top: 2rem;
  &__element {
    margin: 0 0 var(--spacer-xl) 0;
  }
}

.form form span{
  display: flex;
  align-items: center;
  margin-bottom: 1rem;
}

.form form span label{
  width: 10%;
}

.form form span .form__element{
  width: 90%;
  margin: 0;
}

.form form span .form__element input{
  background: #F2F2F2;
    padding: 13px;
    border: unset;
    border-radius: 15px;
}
.action {
  display: flex;
  align-items: center;
  font: var(--font-weight--light) var(--font-size--base) / 1.6 var(--font-family--secondary);
  & > * {
    margin: 0 0 0 var(--spacer-xs);
  }

  text-align: end;
  justify-content: end;
  margin-bottom: 20px !important;
  margin-top: 2px !important;
}
.action {
  margin: var(--spacer-xl) 0 var(--spacer-xl) 0;
}
.checkbox {
  margin-bottom: var(--spacer-2xl);
}

button{
  text-decoration: none;
}
.bottom {
  color: #808080;
  text-align: right;
  justify-content: end;
  text-decoration: none;
  display: flex;
  font-size: 12px;
  margin-top: 5px;
  margin-bottom: var(--spacer-lg);
  font-size: var(--h3-font-size);
  font-weight: var(--font-weight--semibold);
  font-family: var(--font-family--secondary);
  &__paragraph {
    color: #808080 !important;
    text-decoration: none;
    padding-right: 3px;
    margin: 0 0 var(--spacer-base) 0;
    font-size: 12px;
    @include for-desktop {
      margin: 0;
    }
  }
}

.bottom .sf-button--text {
  text-decoration: none;
  color: #0CAA91 !important;
  font-size: 12px;
}
.thank-you {
  &__paragraph {
    &--bold {
      font-weight: var(--font-weight--semibold);
    }
  }
}

.sf-overlay{
  background: #1d1d1b4f !important;
}

.bar-modal-title{
  padding-bottom: 1rem;
  height: 25px;
  margin: 27px 25px;
  border-bottom: 1px solid #2F9393;
  display: flex;
  align-items: center;
}

.modal-back-btn{
  position: absolute;
  padding-left: 1rem;
  font-size: 20px;
}

.sf-modal__content{
  margin-top: 2rem;
  border-top: 1px solid #2F9393;
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
.form__element{
  margin-bottom: 10px;
}

.form__element.password{
  margin-bottom:  10px;
}

.login-title{
  color: #808080;
  font-size: 20px;
  text-transform: uppercase;
}

.form__button{
  background: #0CAA91;
  padding: 12px 5px;
}

@media(max-width: 720px){
  .sf-modal__container{
    width: 100%;
  }
}

#registration {
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
  padding: 5px;
  width: 100%;
  margin-bottom: 20px;
  justify-content: center;
  display: flex;
}

.page-title h2{
  color: #808080;
  font-size: 15px;
  width: 70%;
}

.page-content{
  display: flex;
  justify-content: center;
}

.page-content .page-content-image{
  width: 70%;
  padding: 10px;
}
.page-content .page-content-image img{
  width: 100%;
  
}
.page-content .page-content-text{
  width: 70%;
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