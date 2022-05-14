<script>
  import { setCookie } from "../utils/utils";
  import { Link, navigate } from "svelte-routing";

  let email = "",
    pass = "",
    error = "";

  async function login() {
    const response = await fetch("https://apidev.kanvas.dev/v2/auth", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        email: email.trim(),
        password: pass.trim(),
      }),
    });

    const data = await response.json();
    if (response.status !== 200) {
      error = data?.errors?.message;
    } else {
      setCookie("token", data?.token, 1);
      navigate("/home");
    }
  }
</script>

<div class="bg-gray-800 w-screen h-screen flex items-center justify-center">
  <div class="py-8 px-10 bg-white flex rounded flex-col">
    <span class="text-2xl mb-2">Login</span>
    {#if error.length > 0}
      <span class="text-lg text-red-600">{error}</span>
    {/if}
    <input
      type="email"
      class="mt-5 mb-2 border-gray-500 rounded-lg border p-2 outline-none"
      placeholder="Email"
      bind:value={email}
      required
    />
    <input
      type="password"
      class="mb-5 border-gray-500 rounded-lg border p-2 outline-none"
      placeholder="Password"
      bind:value={pass}
      required
    />
    <div class="mb-5 flex flex-col">
      <span>If you don't have an account yet, click</span>
      <Link to="/signup" class="text-blue-700">here</Link>
    </div>
    <button
      class="bg-blue-700 rounded-full w-full text-white py-2 hover:bg-blue-600"
      on:click={login}>Login</button
    >
  </div>
</div>
