<script setup>
import { onMounted } from "vue";
import * as forgerock from "@forgerock/javascript-sdk";

forgerock.Config.set({
  redirectUri: window.location.origin,
  serverConfig: {
    baseUrl: "",//AM instance url
    timeout: 15000
  },
  realmPath: "",
  tree: ""
});

const handleStep = async (step) => {
  if (step.getCallbackOfType("RedirectCallback")) forgerock.FRAuth.redirect(step);
};

onMounted(async () => {
  const searchParams = new URLSearchParams(window.location.search);
  if (searchParams.has("code") && searchParams.has("state")) {
    forgerock.FRAuth.resume(window.location.href); // Should respond with success and set the session cookie
  } else {
    forgerock.FRAuth.next(step).then(handleStep).catch((err) => {
      console.log(err);
    });
  }
});
</script>

<template>
  <h1>Social Login Test</h1>

  <p>
    This is a simple bare minimum setup to test Social Login for the service
    tree configured
  </p>
  <p>
    Please use Browser's Developer's network tool to monitor the calls towards
    the authentication service tree
  </p>
</template>
