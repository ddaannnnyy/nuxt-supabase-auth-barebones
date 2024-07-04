<template>
  <div>
    <h2>Heres some information pulled from Supabase</h2>
    <p>Check the console for the output of <code>useSupabaseSession()</code> and <code>useSupabaseUser()</code> to see what these objects do</p>
    <h3>Supabase Session</h3>
    <pre>{{ supabaseSession }}</pre>
    <h3>Supabase User</h3>
    <pre>{{ supabaseUser }}</pre>
    <h3>And here is are some examples of useful snippets</h3>
    <p>Is Authenticated? <pre>{{ returnIsAuthenticated }}</pre></p>
    <p>Here is the actual is Authenticated returned: <pre>{{ isAuthenticated }}</pre></p>
    <p>Get Bearer Token <pre>{{ returnBearerToken }}</pre></p>
    <p>Here is the actual bearer token generated: <pre>{{ bearerToken }}</pre></p>
  </div>
</template>

<script setup lang="ts">
const supabaseSession = useSupabaseSession();
const supabaseUser = useSupabaseUser();
const supabase = useSupabaseClient();

const isAuthenticated = ref(false);
const bearerToken = ref('');

isAuthenticated.value = await returnIsAuthenticated();
bearerToken.value = await returnBearerToken();

console.log("Supabase Session\r\n", supabaseSession.value);
console.log("Supabase User\r\n", supabaseUser.value);

async function returnIsAuthenticated() {
  const { data, error } = await supabase.auth.getUser();
  if (error) {
    return false;
  }
  if (data.user) {
    const isAuthenticated = data.user.aud === 'authenticated';
    return isAuthenticated;
  } else return false;
}

async function returnBearerToken() {
  const isAuthenticated = await returnIsAuthenticated();
  if (isAuthenticated && supabaseSession.value != null) {
    const bearerToken = `Bearer ${supabaseSession.value.access_token}`;
    return bearerToken;
  } else {
    throw new Error("Unable to generate Bearer Token");
  }
}

</script>

<style scoped></style>
