<script setup>
import UserInput from "../GithubInput/UserInput.vue"
import GithubOutput from "../GithubOutput/GithubOutput.vue"
import {ref} from "vue" ; 

const userProfile = ref(null);
const error = ref(null);

const getUserProfile = async (username) => {
  try {
    const response = await fetch(
      `https://api.github.com/users/${username}`
    );
    const data = await response.json();

    if (response.ok) {
      userProfile.value = data;
      error.value = null;
    } else {
      userProfile.value = null;
      error.value = `Error: ${data.message}`;
    }
  } catch (err) {
    console.error("Error fetching data:", err);
    error.value = "An error occurred while fetching data.";
  }
};

async function handleSubmit(username) {
  const user = await getUserProfile(username);
}
</script>
<template>
    <div class="container">
      <h1>Github Profile Finder</h1>
      <UserInput  @username-submitted="handleSubmit"/>
      <GithubOutput :userProfile="userProfile" :error="error"/>
    </div>
  </template>
  
  <style scoped>
  .container {
    padding-top: 20px;
    text-align: center;
  }
  
  </style>
