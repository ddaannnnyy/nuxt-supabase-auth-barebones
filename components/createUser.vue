<template>
  <div class="form-wrapper">
    <h2>Create User</h2>
    <form class="form-container" @submit.prevent="createUser" ref="form">
      <label for="email">
        Email
        <input type="email" name="email" v-model="email" required />
      </label>
      <label for="password">
        Password
        <input type="text" name="password" v-model="password" required />
      </label>
      <label for="phone">
        Mobile Phone
        <input type="tel" name="phone" v-model="phone" />
      </label>
      <label for="firstname">
        First Name
        <input type="text" name="firstname" v-model="firstName" required />
      </label>
      <label for="firstname">
        Last Name
        <input type="text" name="lastname" v-model="lastname" required />
      </label>
      <label for="wlthId">
        WlthId
        <input type="text" name="wlthId" v-model="wlthId" readonly />
      </label>
      <button>Generate User</button>
    </form>
  </div>
  <div class="result" v-if="result">
    <pre>
        {{ result }}
    </pre>
  </div>
</template>

<script setup lang="ts">
const supabase = useSupabaseClient();

const form = ref(null as HTMLFormElement | null);
const email = ref("");
const password = ref("");
const firstName = ref("");
const lastname = ref("");
const phone = ref("");
const wlthId = ref("");
const result = ref(undefined as Object | undefined);

onMounted(() => {
  generateWlthId();
});

async function createUser() {
  const { data, error } = await supabase.auth.signUp({
    email: email.value,
    password: password.value,
    options: {
      data: {
        firstName: firstName.value,
        lastname: lastname.value,
        phone: phone.value,
        wlthId: wlthId.value,
        groups: ["Persons", "Accredited Brokers"],
        scopes: ["openid"],
        preferences: {
          quickActions: [
            "brokerDeclaration",
            "privacyDeclarationForm",
            "verificationOfIdForm",
            "postcodeLookupTool",
            "productSwitchRequest",
            "oceanLoanFeesOverview",
          ],
        },
      },
    },
  });
  if (data) {
    result.value = {
      result: `Created ${email.value} successfully`,
      ...data,
    };
    setTimeout(() => {
      if (form.value) {
        form.value.reset();
      }
    }, 3000);
  }
  if (error) {
    result.value = error;
  }
}
function generateWlthId() {
  let result = "";
  const characters =
    "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
  for (let index = 0; index < 20; index++) {
    result += characters.charAt(Math.floor(Math.random() * characters.length));
  }
  wlthId.value = result;
}
</script>

<style scoped>
h2 {
  margin: 0;
  padding: 0;
  padding-bottom: 15px;
}
.form-wrapper {
  font-family: Arial, Helvetica, sans-serif;
  width: 500px;
  border: 1px solid #000;
  border-radius: 4px;
  padding: 30px;
}
.form-container {
  display: flex;
  flex-flow: column nowrap;
  align-items: start;
  gap: 15px;
  width: 300px;
}
.form-container > label {
  display: flex;
  flex-flow: row nowrap;
  gap: 5px;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}
</style>
