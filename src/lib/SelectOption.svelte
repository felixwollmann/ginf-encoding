<script>
    export let encodingString = " ABCDEFGHIJKLMNOPQRSTUVWXYZ.,:-#";
    let encodingName = "original-encoding";

    let customEncodingString = "";

    $: encodingString = toEncodingString(encodingName, customEncodingString);

    function toEncodingString(name, custom) {
        if (name == "original-encoding") {
            return " ABCDEFGHIJKLMNOPQRSTUVWXYZ.,:-#";
        } else if (name == "lisa-encoding") {
            return " AaBbCcDdEeFfGgHhIiJjKkLlMmNnOoPpQqRrSsTtUuVvWwXxYyZz.,:-#ÄäÜüÖö^)(/";
        } else {
            return custom;
        }
    }
</script>

<form class="outer">
    <label for="original-encoding"
        ><input
            bind:group={encodingName}
            type="radio"
            id="original-encoding"
            name="encoding"
            value="original-encoding"
        />GINF-Blatt</label
    >

    <label for="lisa-encoding"
        ><input
            bind:group={encodingName}
            type="radio"
            id="lisa-encoding"
            name="encoding"
            value="lisa-encoding"
        />daslima.eu</label
    >

    <label for="custom-encoding"
        ><input
            bind:group={encodingName}
            type="radio"
            id="custom-encoding"
            name="encoding"
            value="custom-encoding"
        />Benutzerdefiniert</label
    >

    {#if encodingName == "custom-encoding"}
        <textarea
            bind:value={customEncodingString}
            name="custom-input"
            id="custom-input"
        />
    {/if}
</form>

<style>
    form.outer {
        display: flex;
        flex-direction: column;
    }
    textarea {
        width: 100%;
        resize: none;
        outline: none;
        height: 10ch;
        margin-top: 10px;
        outline: none;


        border: 4px solid #da4167;
        border-radius: 10px;
        padding: 10px;
        transition-duration: 200ms;
    }

    textarea:focus {
        border-color: #832161;
    }

    label {
        display: block;
        padding: 10px;
        background-color: hsl(252, 19%, 100%);
        margin: 5px 0px;
        border-radius: 10px;
        box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;

    }

    input {
        margin-right: 10px;
    }
</style>
