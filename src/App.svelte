<script>
import Router from "./Router.svelte";
import {navigate} from "svelte-routing";
import { getCookie } from "./utils/utils";


  
$: isLogged = getCookie("token") ? true : false;

$: if (isLogged) {
  navigate("/home", { replace: false });
} else {
  navigate("/login", { replace: false });
}


</script>


<Router />
