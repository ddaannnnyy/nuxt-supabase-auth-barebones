<template>
  <div>logout page</div>
</template>

<script setup lang="ts">
const supabase = useSupabaseClient();
const { error } = await supabase.auth.signOut();
if (error) {
  console.warn(error);
} else {
  navigateTo("/login");
}
</script>

<style scoped></style>
