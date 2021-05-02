<script>
    import { io } from "socket.io-client";
    const socket = io(`${import.meta.env.VITE_URL}`, {secure:true})

    let name
    let page = 0
    let msg = ''
    export let id 

    function handleMsg () {
        if (page == 0) {
            page = 1
        } else {
           socket.emit('send-message', {
               id,
               name,
               msg
           })
           msg = ''
        }
    }
</script>

<div class="send-area">
    {#if page==0 }
    <input bind:value={name} class="name" type="text" placeholder="Enter name" />
    {:else}
    <textarea placeholder="enter message" bind:value={msg} class="typing-area" cols="30" ></textarea>
    {/if}
    <img on:click={handleMsg} src="https://img.icons8.com/plumpy/50/000000/filled-sent.png" alt="send" />
</div>

<style>

    .send-area {
        height: 14vh;
        position: fixed;
        bottom: 20px;
        width: 90%;
        left: 50%;
        transform: translateX(-50%);
        display: flex;
        justify-content: space-around;
        align-items: center;
    }

    .name {
        padding: 10px;
        height: 5vh;
        width: 60%;
    }

    .typing-area {
        width: 70%;
        position: relative;
        height: 10vh;
    }

</style>