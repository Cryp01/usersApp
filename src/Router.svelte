<script>
  import { Router, Route } from "svelte-routing";
  import Register from "./views/Register.svelte";
  import Login from "./views/Login.svelte";
  import Lists from "./views/Lists.svelte";

</script>

<Router url="">
  <Route path="/login" component={Login} />
  <Route path="/signup" component={Register} />
  <Route path="/home" component={Lists} />
</Router>
