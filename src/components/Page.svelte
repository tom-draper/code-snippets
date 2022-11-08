<script lang="ts">
  import { onMount } from "svelte";
  import hljs from "highlight.js";
  import "highlight.js/styles/github.css";
  import "highlight.js/styles/atom-one-dark.css";
  import ColorPicker from "svelte-awesome-color-picker";
  import { Button, Dropdown, Chevron, Radio } from "flowbite-svelte";

  function titleCase(str: string): string {
    return str.toLowerCase().split(' ').map(function(word) {
      return word.replace(word[0], word[0].toUpperCase());
    }).join(' ');
  }

  function renderCode() {
    let snippet = document.getElementById("code");
    if (snippet != null) {
      snippet.innerHTML = code;
      hljs.highlightElement(snippet);
    }
  }

  function setHeight() {
    let area = document.getElementById('codeSnippet');
    if (area != null) {
      let lines = code.match(/\n/g).length + 1;
      area.rows = lines;
    }
  }

  let borderRadius = 10;
  let terminalBackground = "#25323a";
  let background = "#bfe4f7";

  let showLanguage = true;
  let fontSize = 16;
  let padding = 40;
  let code = "from dataclass import dataclass\nimport numpy as np";
  $: code && renderCode();
  $: code == "" && renderCode();
  let language = "python";
  let font = "Fira Code";
  let theme = "atom-one-dark";

  setHeight();
  onMount(() => {
    renderCode();
  });
</script>

<div>
  <div class="page-content">
    <div class="viewer" style="background: {background};">
      <div
        class="code-snippet-container"
        style="border-radius: {borderRadius}px; font-size: {fontSize}px;"
      >
        <pre style="font-size: {fontSize}px;"><code
            id="code"
            class="language-{language} noselect"
            style="font-family: {font} !important; padding: {padding}px;"
            >{code}</code></pre>
        <textarea
          bind:value={code}
          on:input={setHeight}
          id="codeSnippet"
          name="code-snippet noselect"
          style="background: {terminalBackground};  border-radius: {borderRadius}px; font-family: {font} !important; padding: {padding}px;"
        />
        <div
            style="{showLanguage ? '' : 'display: none;'} right: {padding}px; bottom: {padding}px; font-family: {font};"
            class="language">{language}</div>
      </div>
    </div>
    <div class="controller">
      <ColorPicker bind:hex={background} />
      <ColorPicker bind:hex={terminalBackground} />

      <div class="select-font">

        <Button><Chevron>Font: {font}</Chevron></Button>
        <Dropdown>
          <li>
            <Radio name="group1" bind:group={font} value={"Fira Code"}
              >Fira Code</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"SF Mono"}
              >SF Mono</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"Roboto Mono"}
              >Roboto Mono</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"Space Mono"}
              >Space Mono</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"Ubuntu Mono"}
              >Ubuntu Mono</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"JetBrains Mono"}
              >JetBrains Mono</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"Noto Sans Mono"}
              >Noto Sans Mono</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"IBM Plex Mono"}
              >IBM Plex Mono</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"Share Tech Mono"}
              >Share Tech Mono</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"Cascadia Code"}
              >Cascadia Code</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"Red Hat Mono"}
              >Red Hat Mono</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"Deja Vu Sans Mono"}
              >Deja Vu Sans Mono</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"Hack"}
              >Hack</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"Proggy"}
              >Proggy</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"Terminus"}
              >Terminus</Radio
            >
          </li>
          <li>
            <Radio name="group1" bind:group={font} value={"Dina"}
              >Dina</Radio
            >
          </li>
        </Dropdown>
      </div>

      <div class="select-theme">
        <Button><Chevron>Theme: {titleCase(theme.replace(/-/g, ' '))}</Chevron></Button>
        <Dropdown>
          <li>
            <Radio name="group2" bind:group={theme} value={"atom-one-dark"}
              >Atom One Dark</Radio
            >
          </li>
          <li>
            <Radio name="group2" bind:group={theme} value={"github"}
              >GitHub Light</Radio
            >
          </li>
        </Dropdown>
      </div>

      <div class="show-language">
        <label>
          Show language:
          <input type=checkbox bind:checked={showLanguage}>
        </label>
      </div>

      <div class="select-font-size">
        <button class="increase-font-size-btn" on:click="{() => {fontSize = Math.max(fontSize - 1, 9)}}">-</button>
        <div class="font-size-display">{fontSize}</div>
        <button class="decrease-font-size-btn" on:click="{() => {fontSize = Math.min(fontSize + 1, 25)}}">+</button>
      </div>

      <div class="select-border-radius">
        <div>Margin: {padding-6}</div>
        <input type="range" name="" id="" min="6" max="100" bind:value={padding}>
      </div>
      <div class="select-border-radius">
        <div>Border radius: {borderRadius}</div>
        <input type="range" name="" id="" min="0" max="80" bind:value={borderRadius}>
      </div>

    </div>
  </div>
</div>

<style scoped>
  @font-face {
      font-family: 'Cascadia Code';
      src: url('fonts/CascadiaCode.ttf') format('truetype');
      font-weight: normal;
      font-style: normal;
  }
  @font-face {
      font-family: 'Deja Vu Sans Mono';
      src: url('fonts/DejaVuSansMono.ttf') format('truetype');
      font-weight: normal;
      font-style: normal;
  }
  @font-face {
      font-family: 'Hack';
      src: url('fonts/Hack-Regular.ttf') format('truetype');
      font-weight: normal;
      font-style: normal;
    }
    @font-face {
        font-family: 'Monoid';
        src: url('fonts/Monoid-Regular.ttf') format('truetype');
        font-weight: normal;
        font-style: normal;
    }
    @font-face {
        font-family: 'Proggy';
        src: url('fonts/ProggyClean.ttf') format('truetype');
        font-weight: normal;
        font-style: normal;
    }
    @font-face {
        font-family: 'Terminus';
        src: url('fonts/TerminusTTF-4.49.2.ttf') format('truetype');
        font-weight: normal;
        font-style: normal;
    }
    @font-face {
        font-family: 'Dina';
        src: url('fonts/DinaRemasterCollection.ttc');
        font-weight: normal;
        font-style: normal;
    }
    @font-face {
        font-family: 'SF Mono';
        src: url('fonts/SFMonoRegular.otf');
        font-weight: normal;
        font-style: normal;
    }

  :global(body) {
    margin: 0;
  }
  .page-content {
    display: flex;
    height: 100vh;
  }
  .viewer {
    flex-grow: 1;
    display: grid;
    place-items: center;
  }
  .controller {
    width: 400px;
  }
  #codeSnippet {
    color: transparent;
    background: rgb(176, 176, 176);
    border: none;
    resize: none;
    caret-color: white;
    width: -webkit-fill-available;
    outline: none;
    font-size: inherit;
  }

  #codeSnippet:hover ~ .language {
      opacity: 1;
  }
  .language:hover {
      opacity: 1;
  }

  :global(pre) {
    position: absolute !important;
  }

  .noselect {
  -webkit-touch-callout: none; /* iOS Safari */
    -webkit-user-select: none; /* Safari */
     -khtml-user-select: none; /* Konqueror HTML */
       -moz-user-select: none; /* Old versions of Firefox */
        -ms-user-select: none; /* Internet Explorer/Edge */
            user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}

  pre {
    pointer-events: none;
    margin: 0;
    width: fit-content;
    font-size: inherit;
  }
  pre code {
    padding: 0;
    font-size: inherit;
  }

  .code-snippet-container {
    position: relative;
  }

  pre,
  .code-snippet-container {
    width: 55em;
  }

  #code {
    background: transparent !important;
    white-space: pre-wrap;
  }

  .language {
    position: absolute;
    bottom: 0;
    right: 0;
    font-size: 0.8em;
    color: rgba(255, 255, 255, 0.4);
    opacity: 0;
    user-select: none;
    transition-timing-function: ease;
    transition-duration: 0.08s;
    cursor: text;
  }

  :global(button) {
    display: inline-flex;
    text-align: center;
    text-justify: center;
    border-radius: 6px;
    place-items: center;
  }

  :global(ul) {
    list-style-type: none;
    padding-left: 0.3em;
    margin: 0;
  }

  :global(input[type="color" i]) {
    flex-shrink: none;
    outline: none;
    border-radius: 0;
  }

  .select-font-size {
    display: flex;
  }
  .font-size-display {
    text-align: center;
    margin: 0 5px;
  }
</style>
