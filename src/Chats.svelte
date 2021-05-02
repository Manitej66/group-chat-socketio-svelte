<script>
    import { io } from "socket.io-client";
	import { beforeUpdate, afterUpdate } from 'svelte';
    const socket = io(`${import.meta.env.VITE_URL}`, {secure:true})
    
    export let id
    let div;
    let autoscroll;
    
    let chats = []

    beforeUpdate(() => {
		autoscroll = div && (div.offsetHeight + div.scrollTop) > (div.scrollHeight - 20);
	});

	afterUpdate(() => {
        if (autoscroll) div.scrollTo(0, div.scrollHeight);
	});

    socket.on('send-all', data => {
        chats = [...chats, data];
    })

</script>

<div class="chats-area" bind:this={div}>
    {#if chats.length > 0}
        {#each chats as chat}
            <div class="{chat.id === id ? 'out-msg' : 'in-msg'}">
                <p style="padding: 4px; font-style: italic;opacity:0.8">{chat.name} says..</p>
                <p style="font-weight: bold;padding-left: 4px;"> {chat.msg}</p>
            </div>
        {/each}
    {:else}
        <p>No messages here yet !</p>
    {/if}
</div>

<style>

    .chats-area {
        width: 90%;
        border: 1px solid #091A7A;
        height: 70vh;
        margin: 10vh auto 5vh auto;
        padding: 4px;
        display: flex;
        flex-direction: column;
        overflow-y: scroll;
    }

    p {
        margin: 0;
    }

    .in-msg {
        background-color: lightgray;
        margin: 10px 0;
        padding: 3px;
        border-radius: 1px 10px ;
        max-width: 70%;
        align-self: flex-start;
    }

    .out-msg {
        background-color: lightgray;
        margin: 10px 0;
        padding: 5px;
        border-radius: 10px 1px ;
        max-width: 70%;
        align-self: flex-end;
        background-color: #0B4E74;
        color: white;
    }

    #btm {
        height: 10px;
        background-color: red;
        display: inline-block;
    vertical-align:bottom;
    }
</style>