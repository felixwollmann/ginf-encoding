<script>
    export let output;

    let success = false;

    async function copy() {
        try {
            await navigator.clipboard.writeText(output);
            success = true;
            setTimeout(() => success = false, 2_000);
        } catch (err) {
            console.error(err);
        }
    }
</script>

<div class="icon-container" on:click={copy} class:success>
    <span class="material-icons"> {success ? 'done' : 'content_copy'} </span>
</div>

<style>
    div.icon-container {
        text-align: center;
        /* padding: 10px; */
        /* height: 100%;
    width: 100%; */
        background-color: #da4167;
        color: white;
        /* aspect-ratio: 1/1; */
        width: auto;
        /* height: 100%; */
        /* height: auto; */
        flex: 2;
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 10px;

        cursor: pointer;
        transition: background-color 0.2s;
    }

    div.icon-container:hover {
        background-color: #832161;
    }

    div.icon-container.success {
        background-color: #20BF55;
    }
</style>
