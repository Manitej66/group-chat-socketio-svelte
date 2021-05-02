<script>
  import { io } from "socket.io-client";
  import Nav from "./Nav.svelte";
  const socket = io(`${import.meta.env.VITE_URL}`, {secure:true})
  import './App.css'
  import Chats from "./Chats.svelte";
  import Send from "./Send.svelte";

   let id

  socket.on('connect', () => {
        id = socket.id
    })
  
</script>

<div>
  <Nav/>
  <Chats id={id} />
  <Send id={id} />
</div>

