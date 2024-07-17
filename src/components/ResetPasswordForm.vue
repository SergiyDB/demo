<template>
  <div class="column q-pa-md q-gutter-y-md" style="width: 500px; max-width: 800px">

    <q-avatar size="120px">
      <img src="src/assets/images/logos/robo_logo.jpg">
    </q-avatar>

    <div class="text-h3 text-weight-bold">Forgot password?</div>
    <div class="text-body1">Fill the form to reset your password</div>

    <q-form
      @submit="onSubmit"
      class="q-gutter-sm"
    >
      <q-input
        filled
        type="email"
        v-model="email"
        label="Email address *"
        lazy-rules
        :rules="[
          (val) => !!val || 'Email required!' ,
          (val, rules) => rules.email(val) || 'Please enter a valid email address'
        ]"
      />

      <div class="text-body1">Return to <span class="text-blue text-weight-bold" style="cursor: pointer" @click="navToSignIn">Sign In</span></div>

      <div class="q-py-md">
        <q-btn class="full-width" size="1rem" label="Send reset link" type="submit" color="primary"/>
      </div>
    </q-form>

  </div>
</template>

<script setup>
import { useQuasar } from 'quasar'
import { useRouter } from 'vue-router';
import { ref } from 'vue';
import AuthService from 'src/services/auth_service';
// import { getState, setUser } from 'src/app_state/app_state';


const $q = useQuasar()
const router = useRouter();

const authService = new AuthService();

const email = ref(null)

const onSubmit = async () => {

  try {
    const result = await authService.resetUserPassword(email.value.trim())

    if (result.status === 'ok') {

      // setUser(result.user);

      // const { user } = getState();
      // const userType = user?.user_type;

      // if (userType === 'school') {
      //   router.push('/school');
      // } else if (userType === 'company') {
      //   router.push('/company');
      // } else {
      //   router.push('/');
      // }

      navToSignIn();

      $q.notify({
        color: 'green-4',
        textColor: 'white',
        icon: 'cloud_done',
        message: 'Submitted'
      });

    } else {

      $q.notify({
        color: 'red-5',
        textColor: 'white',
        icon: 'warning',
        message: 'Invalid email'
      });

      onReset();

    }

  } catch (error) {
    console.error(error)
    $q.notify({
      color: 'red-5',
      textColor: 'white',
      icon: 'warning',
      message: error.message,
    });

    onReset();

  }


  // if (accept.value !== true) {
  //   $q.notify({
  //     color: 'red-5',
  //     textColor: 'white',
  //     icon: 'warning',
  //     message: 'You need to accept the license and terms first'
  //   })
  // }
  // else {
  //   $q.notify({
  //     color: 'green-4',
  //     textColor: 'white',
  //     icon: 'cloud_done',
  //     message: 'Submitted'
  //   })
  // }
};

const onReset = () => {
  email.value = null
  // password.value = null
  // rememberMe.value = false
}

const navToSignIn = () => {
  router.push('/auth/sign-in');
}

</script>
