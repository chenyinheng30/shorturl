<script setup lang="ts">
defineProps({sitekey: String})
const script = document.createElement("script");
script.src = "https://challenges.cloudflare.com/turnstile/v0/api.js";
document.head.appendChild(script);
</script>

<template>
    <div class="cf-turnstile" :data-sitekey="sitekey"></div>
</template>