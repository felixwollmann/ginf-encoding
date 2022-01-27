<script>
  import CopyButton from "./lib/CopyButton.svelte";
  import SelectOption from "./lib/SelectOption.svelte";

  const map = new Map();

  let encodingString = " ABCDEFGHIJKLMNOPQRSTUVWXYZ.,:-#";

  $: length = getLength(encodingString.length);

  $: shouldUpperCase = encodingString == " ABCDEFGHIJKLMNOPQRSTUVWXYZ.,:-#";

  function getLength(length) {
    if (length <= 0) length = 1;
    return Math.ceil(Math.log2(length));
  }

  $: {
    map.clear();
    for (let i = 0; i < encodingString.length; i++) {
      map.set(i, encodingString[i]);
    }
  }

  /**
   *
   * @param {String} input
   * @returns {number[]}
   */
  function fromString(input) {
    const output = [];
    if (shouldUpperCase) {
      input = input.toUpperCase();
    }
    Array.from(input).forEach((char) => {
      for (const [key, val] of map.entries()) {
        if (char == val) {
          output.push(key);
          return;
        }
      }
    });
    return output;
  }

  /**
   * @param {string} input
   */
  function toArray(input) {
    const array = [];
    input = input.replaceAll(/[^01]/g, "");

    for (let i = 0; i < input.length; i += length) {
      array.push(parseInt(input.substring(i, i + length), 2));
    }

    return array;
  }

  function toString(input) {
    let output = "";
    input.forEach((val) => {
      const char = map.get(val);
      if (char != undefined) {
        output += char;
      }
    });
    return output;
  }

  function arrayToString(input) {
    return input
      .map(
        (val) => "0".repeat(length - val.toString(2).length) + val.toString(2)
      )
      .join(" ");
  }

  function escapeRegExp(text) {
    for (let char of "[]{}()\\^$.|?*+-") {
      text = text.replace(char, "\\" + char);
    }
    return text;
    // return text.replace();
  }

  let codeInput;
  let text;
</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.1/normalize.min.css"
    integrity="sha512-NhSC1YmyruXifcj/KFRWoC561YpHpc5Jtzgvbuzx5VozKpWvQ+4nXhPdFgmx8xqexRcpAglTj9sIBWINXa8x5w=="
    crossorigin="anonymous"
    referrerpolicy="no-referrer"
  />
  <link
    href="https://fonts.googleapis.com/css2?family=Quicksand:wght@500&display=swap"
    rel="stylesheet"
  />
  <link
    href="https://fonts.googleapis.com/icon?family=Material+Icons"
    rel="stylesheet"
  />
</svelte:head>

<main>
  <h1>Binärcode &lt;-&gt; Text</h1>

  <label for="codeInput">Binärcode</label>
  <div class="input-container">
    <input
      pattern={`(([01]){${length}} ?)*`}
      type="text"
      bind:value={codeInput}
      on:input={() => {
        text = toString(toArray(codeInput));
      }}
      placeholder="Binärcode"
      id="codeInput"
    />
    <CopyButton output={codeInput} />
  </div>
  <br />
  <label for="text">Text</label>
  <div class="input-container">
    <input
      type="text"
      pattern={`[${escapeRegExp(encodingString)}]*`}
      bind:value={text}
      on:input={() => {
        codeInput = arrayToString(fromString(text));
      }}
      placeholder="Text"
      id="text"
    />
    <CopyButton output={text} />
  </div>

  <SelectOption bind:encodingString />
</main>

<style>
  main {
    background-color: #eee;
    border-radius: 15px;
    padding: 2rem;
    /* width: 400px; */
    margin: 2rem;
    background-color: #f0eff4;
  }

  h1 {
    margin-top: 0;
    color: #da4167;
    font-family: "Quicksand", sans-serif;
  }

  :global(html) {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
    background-color: #3d2645;
  }

  :global(*) {
    box-sizing: border-box;
    font-family: "Quicksand", sans-serif;
  }

  input {
    flex: 8;
    outline: none;
    font-size: larger;
    border: 4px solid #da4167;
    border-radius: 10px;
    padding: 10px;
    width: 100%;
    transition-duration: 200ms;
  }

  div.input-container {
    width: 100%;
    display: flex;
    /* justify-content: center; */
    align-items: stretch;
    margin: 0px 0 15px;
    gap: 0.2rem;
  }

  input:invalid {
    color: red;
  }

  input:focus {
    border-color: #832161;
  }

  label {
    font-weight: bold;
    font-size: small;
    margin-left: 3px;
  }
</style>
