<script>
  import { Link, navigate } from "svelte-routing";
  import { setCookie } from "../utils/utils";

  let firstname = "",
    lastname = "",
    company = "",
    email = "",
    pass = "",
    verifypass = "",
    error = "";

  $: matchpassword = pass === verifypass;

  async function Signup() {
    if (matchpassword) {
      const response = await fetch("https://apidev.kanvas.dev/v2/users", {
        headers: {
          "Content-Type": "application/json",
        },
        method: "POST",
        body: JSON.stringify({
          firstname: firstname.trim(),
          lastname: lastname.trim(),
          email: email.trim(),
          password: pass.trim(),
          verify_password: verifypass.trim(),
          default_company: company.trim(),
        }),
      });

      const data = await response.json();

      if (response.status !== 200) {
        error = data?.errors.message;
      } else {
        setCookie("token", data?.session?.token, 1);
        navigate("/home", { replace: true });
      }
    }
  }
</script>

<div class="bg-gray-800 w-screen h-screen flex items-center justify-center">
  <div class="py-8 px-10 bg-white flex rounded flex-col">
    <span class="text-2xl mb-4">Register</span>
    {#if error}
    <span class="text-lg text-red-600">{error}</span>
    {/if}
    <input
      type="text"
      class="mb-2 border-gray-500 rounded-lg border p-2 outline-none"
      placeholder="First name"
      bind:value={firstname}
      required
    />
    <input
      type="text"
      class="mb-2 border-gray-500 rounded-lg border p-2 outline-none"
      placeholder="Last name"
      bind:value={lastname}
      required
    />
    <input
      type="text"
      class="mb-2 border-gray-500 rounded-lg border p-2 outline-none"
      placeholder="Company"
      bind:value={company}
      required
    />

    <input
      type="email"
      class="mb-2 border-gray-500 rounded-lg border p-2 outline-none"
      placeholder="Email"
      bind:value={email}
      required
    />
    <input
      type="password"
      class="mb-2 border-gray-500 rounded-lg border p-2 outline-none"
      placeholder="Password"
      bind:value={pass}
      required
    />
    <input
      type="password"
      class="mb-5 border-gray-500 rounded-lg border p-2 outline-none {!matchpassword &&
        'border-red-700'}"
      placeholder="Verify Password"
      bind:value={verifypass}
      required
    />

    <div class="mb-5 flex flex-col">
      <span>If you already have an account click</span>
      <Link to="/login" class="text-blue-700">here</Link>
    </div>
    <button
      class="bg-blue-700 rounded-full w-full text-white py-2 hover:bg-blue-600"
      on:click={Signup}>Sign up</button
    >
  </div>
</div>
