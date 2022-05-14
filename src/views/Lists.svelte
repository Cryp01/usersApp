<script>
  import { getCookie } from "../utils/utils";
  import { navigate } from "svelte-routing";
  import { onMount } from "svelte/internal";

  let token = getCookie("token");
  let listUser = [];
  let error = "";

  $: token ?? navigate("/login");


  function logout(){
    document.cookie = "token=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;";
    navigate("/login");
  }

  onMount(async () => {
    const response = await fetch("https://apidev.kanvas.dev/v2/users", {
      method: "GET",
      headers: {
        Authorization: token,
      },
    });

    const data = await response.json();

    if (response.status !== 200) {
      error = data?.errors?.message;
    } else {
      for (let user of data) {
        listUser.push({
          firstname: user?.firstname,
          lastname: user?.lastname,
          email: user?.email,
          company: user?.default_company,
          createAt: user?.registered,
          updateAt: user?.updated_at
        });

        listUser = listUser;
      }
    }
  });
</script>

<div class="bg-gray-800 w-screen h-screen flex items-center justify-center">
  <div class="py-8 px-10 bg-white flex rounded flex-col">
    <div class="flex flex-row justify-between">
      <span class="text-xl">List Users</span>
      <span class="cursor-pointer" on:click={logout}>logout</span>
    </div>
    {#if error.length > 0}
      <span class="text-lg text-red-600">{error}</span>
    {/if}
    <div class="my-2 flex flex-col overflow-hidden overflow-y-scroll max-h-custom px-5" >
      {#each listUser as user}
        <div class="my-2 flex flex-col border border-gray-400 p-5">
          <span>firstname: {user?.firstname}</span>
          <span>lastname: {user?.lastname}</span>
          <span>email: {user?.email}</span>
          <span>company: {user?.company} </span>
          <span>createAt: {user?.createAt}</span>
          <span>updateAt: {user?.updateAt}</span>
        </div>
      {/each}
    </div>
  </div>
</div>
<style>
  .max-h-custom{
    max-height: 75vh;
  }
</style>