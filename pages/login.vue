<template>
  <div>
    <form @submit.prevent="handleLogin">
      <label for="email">
        Email
        <input type="text" name="email" required v-model="email" />
      </label>
      <label for="password">
        Password
        <input type="password" name="password" required v-model="password" />
      </label>
      <label for="preventSignIn">
        Prevent Sign In and print the response?
        <input type="checkbox" name="preventSignIn" v-model="preventSignIn" />
      </label>
      <p v-if="submitData">
        Data: 
          <pre>
            {{ submitData }}
          </pre>
      </p>
      <p v-if="errorText" style="color: red">
        Error: 
          {{ errorText }}
      </p>
      <button>Login</button>
    </form>
    <div class="tooltip">
      <p>
      This is a basic login system, you can read the documentation
      <a
        href="https://supabase.com/docs/guides/auth/passwords?queryGroups=language&language=js"
        target="_blank"
        >here</a
      >
    </p>
    <p>
      You can use these credentials to log in
      <pre>
        Email: d.hebdon@wlth.com
        Pass: Password
      </pre>
    </p>
    <p>
      It only handles email/pass at the moment, but you can see all of the SSO
      providers
      <a
        href="https://supabase.com/docs/guides/auth#social-auth"
        target="_blank"
        >here</a
      >
    </p>
    <p>
      As well as documentation about magic link or email OTP systems
      <a
        href="https://supabase.com/docs/guides/auth/auth-email-passwordless?queryGroups=language&language=js"
        target="_blank"
        >here</a
      >
    </p>
    <p>
      Alternatively still, phone based authentication
      <a
        href="https://supabase.com/docs/guides/auth/phone-login?queryGroups=language&language=js"
        target="_blank"
        >here</a
      >
    </p>
    </div>
  </div>
</template>

<script setup lang="ts">
const supabase = useSupabaseClient();

const email = ref("");
const password = ref("");
const errorText = ref(undefined as Object | undefined);
const submitData = ref(undefined as Object | undefined);
const preventSignIn = ref(false);

async function handleLogin() {
  console.log("handling form");

  const { data, error } = await supabase.auth.signInWithPassword({
    email: email.value,
    password: password.value,
  });

  if (data) {
    console.log(data);
    submitData.value = data;
  }

  if (error) {
    console.warn(error);
    errorText.value = error;
  }

  if (!preventSignIn.value) {
    navigateTo('/');
  }

}

</script>

<style scoped>
form {
  display: flex;
  flex-flow: column nowrap;
  gap: 20px;
  width: 250px;
}

label {
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-between;
  gap: 20px;
  width: 100%;
}

.tooltip {
  padding-top: 50px;
}

.button-wrapper {
  display: flex;
  flex-flow: row nowrap;
  gap: 30px;
}
</style>
