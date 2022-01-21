<script>
  import CopyButton from "./lib/CopyButton.svelte";

  const map = new Map();

  const values = " ABCDEFGHIJKLMNOPQRSTUVWXYZ.,:-#";

  for (let i = 0; i < 32; i++) {
    map.set(i, values[i]);
  }

  /**
   *
   * @param {String} input
   * @returns {number[]}
   */
  function fromString(input) {
    const output = [];
    Array.from(input.toUpperCase()).forEach((char) => {
      map.forEach((val, key) => {
        if (char == val) {
          output.push(key);
          return;
        }
      });
    });
    return output;
  }

  /**
   * @param {string} input
   */
  function toArray(input) {
    const array = [];
    input = input.replaceAll(/[^01]/g, "");

    for (let i = 0; i < input.length; i += 5) {
      array.push(parseInt(input.substring(i, i + 5), 2));
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
      .map((val) => "0".repeat(5 - val.toString(2).length) + val.toString(2))
      .join(" ");
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
      pattern="(([01])&lbrace;5&rbrace; ?)*"
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
      pattern="[ ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz.,:\-#]*"
      bind:value={text}
      on:input={() => {
        codeInput = arrayToString(fromString(text));
      }}
      placeholder="Text"
      id="text"
    />
    <CopyButton output={text} />
  </div>
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
