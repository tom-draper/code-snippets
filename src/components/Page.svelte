<script lang="ts">
  import { onMount } from "svelte";
  import hljs from "highlight.js";
  // import "highlight.js/styles/github.css";
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

  function setTheme(theme: string) {
    document.documentElement.setAttribute('data-theme', theme);
  }

  let borderRadius = 10;
  let terminalBackground = "#25323a";
  let background = "#bfe4f7";

  let showLanguage = true;
  let showMacOS = false;
  let fontSize = 16;
  let padding = 40;
  let code = "from datetime import datetime\nimport numpy as np";
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
      <div
          style="{showMacOS ? '' : 'display: none;'}; border-radius: {borderRadius}px {borderRadius}px 0 0; background: {terminalBackground}; padding-bottom: {padding}px;"
          class="mac-os">
          <div class="dot red-dot"></div>
          <div class="dot yellow-dot"></div>
          <div class="dot green-dot"></div>
        </div>
        <pre style="font-size: {fontSize}px;"><code
            id="code"
            class="language-{language} noselect"
            style="font-family: {font} !important; padding: {padding}px {padding}px; {showMacOS ? `margin-top: -${padding}px` : ''}"
            >{code}</code></pre>
        <textarea
          bind:value={code}
          on:input={setHeight}
          id="codeSnippet"
          name="code-snippet noselect"
          style="background: {terminalBackground}; {showMacOS ? `border-radius: 0 0 ${borderRadius}px ${borderRadius}px;` : `border-radius: ${borderRadius}px;`} font-family: {font} !important; padding: {padding}px {padding}px; {showMacOS ? 'padding-top: 0' : ''}"
        />
        <div
            style="right: {padding}px; bottom: {padding}px; font-family: {font};"
            class="language {showLanguage ? '' : 'hide'}">{language}</div>
      </div>
    </div>
    <div class="controller">
      <ColorPicker label="Background color" bind:hex={background} />
      <ColorPicker label="Editor color" bind:hex={terminalBackground} />

      <div class="selector">
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

      <div class="selector">
        <Button><Chevron>Theme: {titleCase(theme.replace(/-/g, ' '))}</Chevron></Button>
        <Dropdown>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('atom-one-dark')}}" value="atom-one-dark"
              >Atom One Dark</Radio>
          </li>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('atom-one-light')}}" value="atom-one-light"
              >Atom One Light</Radio>
          </li>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('github')}}" value="github"
              >GitHub Light</Radio>
          </li>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('github-dark')}}"  value="github-dark"
              >GitHub Dark</Radio>
          </li>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('monokai')}}" value="monokai"
              >Monokai</Radio>
          </li>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('nord')}}" value="nord"
              >Nord</Radio>
          </li>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('xcode')}}" value="xcode"
              >XCode</Radio>
          </li>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('googlecode')}}" value="googlecode"
              >Google Code</Radio>
          </li>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('a11y-dark')}}" value="a11y-dark"
              >A11y Dark</Radio>
          </li>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('a11y-light')}}" value="a11y-light"
              >A11y Light</Radio>
          </li>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('night-owl')}}" value="night-owl"
              >Night Owl</Radio>
          </li>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('obsidian')}}" value="obsidian"
              >Obsidian</Radio>
          </li>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('tokyo-night-dark')}}" value="tokyo-night-dark"
              >Tokyo Night Dark</Radio>
          </li>
          <li>
            <Radio name="group2" bind:group={theme} on:click="{() => {setTheme('tokyo-night-light')}}" value="tokyo-night-light"
              >Tokyo Night Light</Radio>
          </li>
        </Dropdown>
      </div>

      <div class="show-language">
        <label>
          Show language:
          <input type=checkbox bind:checked={showLanguage}>
        </label>
      </div>

      <div class="show-os">
        <label>
          MacOS:
          <input type=checkbox bind:checked={showMacOS}>
        </label>
      </div>

      <div class="select-font-size">
        <button class="increase-font-size-btn" on:click="{() => {fontSize = Math.max(fontSize - 1, 9)}}">-</button>
        <div class="font-size-display">{fontSize}</div>
        <button class="decrease-font-size-btn" on:click="{() => {fontSize = Math.min(fontSize + 1, 25)}}">+</button>
      </div>

      <div class="select-border-radius">
        <div>Margin: {padding-10}</div>
        <input type="range" name="" id="" min="10" max="100" bind:value={padding}>
      </div>
      <div class="select-border-radius">
        <div>Border radius: {borderRadius}</div>
        <input type="range" name="" id="" min="0" max="50" bind:value={borderRadius}>
      </div>

    </div>
  </div>
</div>

<style lang="scss" scoped>
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
    height: fit-content;
    min-height: 100vh;
  }
  .controller {
    width: max(300px, 20%);
    padding: 20px 20px;
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
    overflow: hidden;
  }

  #codeSnippet:hover ~ .hide {
      opacity: 1;
  }
  .hide:hover {
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
    margin: 50px 0;
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
    user-select: none;
    transition-timing-function: ease;
    transition-duration: 0.08s;
    cursor: text;
  }

  .hide {
    opacity: 0;
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

  .select-font-size {
    display: flex;
  }
  .font-size-display {
    text-align: center;
    margin: 0 5px;
  }

  .mac-os {
    display: flex;
    padding-top: 20px;
    padding-left: 20px;
  }
  .dot {
    width: 12px;
    height: 12px;
    border-radius: 10px;
    margin: 0 6px;
  }
  .red-dot {
    background: #e67059;
  }
  .yellow-dot {
    background: #fbb665;
  }
  .green-dot {
    background: #5ec851;
  }

  :global(.selector) {
    margin: 10px 0;
    :global(button) {
      width: 100%;
      :global(svg) {
        margin-left: auto;
      }
    }
    :global(div) {
      position: relative !important;
      transform: none !important;
    }
    :global(ui) {
      padding-left: 0;
    }
  }

  :global(.color-picker) {
    margin: 10px 0;
    :global(label) {
      :global(div) {
        border-radius: 0;
      }
    }
  }

  .select-font-size,
  .show-language,
  .show-macos {
    margin: 10px 0;
  }

</style>