<script lang="ts">
  import { onMount } from "svelte";
  import hljs from "highlight.js";
  // import "highlight.js/styles/github.css";
  import html2canvas from "html2canvas";
  import ColorPicker from "svelte-awesome-color-picker";
  import { Button, Dropdown, Chevron, Radio } from "flowbite-svelte";

  function titleCase(str: string): string {
    return str
      .toLowerCase()
      .split(" ")
      .map(function (word) {
        return word.replace(word[0], word[0].toUpperCase());
      })
      .join(" ");
  }

  function renderCode() {
    let snippet = document.getElementById("code");
    if (snippet != null) {
      snippet.innerHTML = code;
      hljs.highlightElement(snippet);
    }
  }

  function setHeight() {
    let area: HTMLTextAreaElement | null = document.getElementById(
      "codeSnippet"
    ) as HTMLTextAreaElement;
    if (area != null) {
      let matches = code.match(/\n/g);
      if (matches != null) {
        let lines = matches.length + 1;
        area.rows = lines;
      }
    }
  }

  function setTheme(theme: string) {
    document.documentElement.setAttribute("data-theme", theme);
  }

  function setOS(os: string) {
    if (os == "Windows") {
      showWindows = true;
      showMacOS = false;
    } else if (os == "MacOS") {
      showWindows = false;
      showMacOS = true;
    } else {
      showWindows = false;
      showMacOS = false;
    }
  }

  function takeScreenshot() {
    let c = document.getElementById("download") as HTMLElement; // or document.getElementById('canvas');
    html2canvas(c).then((canvas: any) => {
      let t = canvas.toDataURL().replace("data:image/png;base64,", "");
      downloadBase64File("image/png", t, "image");
    });
  }

  function downloadBase64File(
    contentType: any,
    base64Data: any,
    fileName: any
  ) {
    const linkSource = `data:${contentType};base64,${base64Data}`;
    const downloadLink = document.createElement("a");
    downloadLink.href = linkSource;
    downloadLink.download = fileName;
    downloadLink.click();
  }

  let borderRadius = 10;
  let terminalBackground = "#25323a";
  let background = "#bfe4f7";

  let showLanguage = true;
  let showMacOS = false;
  let showWindows = false;
  let fontSize = 16;
  let padding = 40;
  let code =
    "from functools import lru_cache\n\n@lru_cache\ndef fibonacci(n):\n    if n in {0, 1}:\n        return n\n    return fibonacci(n-1) + fibonacci(n-2)";
  $: code && renderCode();
  $: code == "" && renderCode();
  let language = "python";


  let fonts = [
    'Fira Code',
    'SF Mono',
    'Hack',
    'Noto Sans Mono',
    'Roboto Mono',
    'Space Mono',
    'Ubuntu Mono',
    'JetBrains Mono',
    'IBM Plex Mono',
    'Share Tech Mono',
    'Cascadia Code',
    'Red Hat Mono',
    'Deja Vu Sans Mono',
    'Proggy',
    'Terminus',
    'Dina',
  ]
  let font = fonts[0];
  let themes = [
    {name: 'Atom One Dark', id: 'atom-one-dark'},
    {name: 'Atom One Light', id: 'atom-one-light'},
    {name: 'GitHub Light', id: 'github'},
    {name: 'GitHub Dark', id: 'github-dark'},
    {name: 'Monokai', id: 'monokai'},
    {name: 'Nord', id: 'nord'},
    {name: 'XCode', id: 'xcode'},
    {name: 'Google Code', id: 'googlecode'},
    {name: 'A11y Dark', id: 'a11y-dark'},
    {name: 'A11y Light', id: 'a11y-light'},
    {name: 'Night Owl', id: 'night-owl'},
    {name: 'Obsidian', id: 'obsidian'},
    {name: 'Tokyo Night Dark', id: 'tokyo-night-dark'},
    {name: 'Tokyo Night Light', id: 'tokyo-night-light'},
  ]
  let theme = "atom-one-dark";

  $: font && setHeight();

  onMount(() => {
    renderCode();
    setHeight();
  });
</script>

<div>
  <div class="page-content">
    <div class="viewer" id="download" style="background: {background};">
      <div
        class="code-snippet-container"
        style="border-radius: {borderRadius}px; font-size: {fontSize}px;"
      >
        <div
          style="{showMacOS
            ? ''
            : 'display: none;'}; border-radius: {borderRadius}px {borderRadius}px 0 0; background: {terminalBackground}; padding-bottom: {padding}px;"
          class="mac-os"
        >
          <div class="dot red-dot" />
          <div class="dot yellow-dot" />
          <div class="dot green-dot" />
        </div>
        <div
          style="{showWindows
            ? ''
            : 'display: none;'}; border-radius: {borderRadius}px {borderRadius}px 0 0; background: {terminalBackground};"
          class="windows"
        >
          <img src="windows.png" alt="" />
        </div>
        <pre
          style="font-family: {font} !important; font-size: {fontSize}px;"><code
            id="code"
            class="language-{language} noselect"
            style="font-family: {font} !important; font-size: {fontSize}px; padding: {padding}px {padding}px; {showMacOS
              ? `margin-top: -${padding}px`
              : ''}">{code}</code
          ></pre>
        <textarea
          bind:value={code}
          on:input={setHeight}
          id="codeSnippet"
          name="code-snippet noselect"
          cols="80"
          style="background: {terminalBackground}; {showMacOS || showWindows
            ? `border-radius: 0 0 ${borderRadius}px ${borderRadius}px;`
            : `border-radius: ${borderRadius}px;`} font-family: {font} !important; padding: {padding}px {padding}px; {showMacOS
            ? 'padding-top: 0'
            : ''}"
        />
        <div
          style="right: {padding}px; bottom: {padding}px; font-family: {font};"
          class="language {showLanguage ? '' : 'hide'}"
        >
          {language}
        </div>
      </div>
    </div>
    <div class="controller">
      <div class="options">
        <ColorPicker label="Background color" bind:hex={background} />
        <ColorPicker label="Editor color" bind:hex={terminalBackground} />
  
        <div class="selector">
          <Button><Chevron>Font: {font}</Chevron></Button>
          <Dropdown>
            {#each fonts as fontFamily}
            <li>
              <Radio name="group1" bind:group={font} value={fontFamily}
                >{fontFamily}</Radio
              >
            </li>
            {/each}
          </Dropdown>
        </div>
  
        <div class="selector">
          <Button
            ><Chevron>Theme: {titleCase(theme.replace(/-/g, " "))}</Chevron
            ></Button
          >
          <Dropdown>
            {#each themes as _theme}
            <li>
              <Radio
                name="group2"
                bind:group={theme}
                on:click={() => {
                  setTheme(_theme.id);
                }}
                value="{_theme.id}">{_theme.name}</Radio
              >
            </li>
            {/each}
          </Dropdown>
        </div>
  

  
        <div class="show-os">
          <button
            class="os-btn none-os-btn {showMacOS || showWindows ? '' : 'active'}"
            on:click={() => setOS("None")}>None</button
          >
          <button
            class="os-btn mac-os-btn {showMacOS ? 'active' : ''}"
            on:click={() => setOS("MacOS")}>MacOS</button
          >
          <button
            class="os-btn windows-btn {showWindows ? 'active' : ''}"
            on:click={() => setOS("Windows")}>Windows</button
          >
        </div>
  
        <div class="select-font-size">
          <div class="select-font-size-label">
            Font size:
          </div>
          <button
            class="increase-font-size-btn noselect"
            on:click={() => {
              fontSize = Math.max(fontSize - 1, 9);
            }}>–</button
          >
          <div class="font-size-display">{fontSize}px</div>
          <button
            class="decrease-font-size-btn noselect"
            on:click={() => {
              fontSize = Math.min(fontSize + 1, 25);
            }}>+</button
          >
        </div>
  
        <div class="select-border-radius">
          <div>Margin: {padding - 10}</div>
          <input
            type="range"
            name=""
            id=""
            min="10"
            max="100"
            bind:value={padding}
          />
        </div>
        <div class="select-border-radius">
          <div>Border radius: {borderRadius}</div>
          <input
            type="range"
            name=""
            id=""
            min="0"
            max="50"
            bind:value={borderRadius}
          />
        </div>
        <div class="show-language">
          <label>
            Show language:
            <input type="checkbox" bind:checked={showLanguage} />
          </label>
        </div>
      </div>
      <div class="take-screenshot">
        <button on:click="{takeScreenshot}">Take Screenshot</button>
      </div>
    </div>
  </div>
</div>

<style lang="scss" scoped>
  @font-face {
    font-family: "Cascadia Code";
    src: url("fonts/CascadiaCode.ttf") format("truetype");
    font-weight: normal;
    font-style: normal;
  }
  @font-face {
    font-family: "Deja Vu Sans Mono";
    src: url("fonts/DejaVuSansMono.ttf") format("truetype");
    font-weight: normal;
    font-style: normal;
  }
  @font-face {
    font-family: "Hack";
    src: url("fonts/Hack-Regular.ttf") format("truetype");
    font-weight: normal;
    font-style: normal;
  }
  @font-face {
    font-family: "Monoid";
    src: url("fonts/Monoid-Regular.ttf") format("truetype");
    font-weight: normal;
    font-style: normal;
  }
  @font-face {
    font-family: "Proggy";
    src: url("fonts/ProggyClean.ttf") format("truetype");
    font-weight: normal;
    font-style: normal;
  }
  @font-face {
    font-family: "Terminus";
    src: url("fonts/TerminusTTF-4.49.2.ttf") format("truetype");
    font-weight: normal;
    font-style: normal;
  }
  @font-face {
    font-family: "Dina";
    src: url("fonts/DinaRemasterCollection.ttc");
    font-weight: normal;
    font-style: normal;
  }
  @font-face {
    font-family: "SF Mono";
    src: url("fonts/SFMonoRegular.otf");
    font-weight: normal;
    font-style: normal;
  }
  @font-face {
    font-family: "SF Pro Display Light";
    src: url("fonts/SF-Pro-Display-Light.otf");
  }
  @font-face {
    font-family: "SF Pro Display Regular";
    src: url("fonts/SF-Pro-Display-Regular.otf");
    font-weight: normal;
    font-style: normal;
  }
  @font-face {
    font-family: "SF Pro Display Medium";
    src: url("fonts/SF-Pro-Display-Medium.otf");
    font-weight: normal;
    font-style: normal;
  }
  @font-face {
    font-family: "SF Pro Display SemiBold";
    src: url("fonts/SF-Pro-Display-Semibold.otf");
  }
  @font-face {
    font-family: "SF Pro Display Bold";
    src: url("fonts/SF-Pro-Text-Bold.otf");
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
    width: max(340px, 20%);
    display: flex;
    flex-direction: column;
    font-family: 'SF Pro Display Regular'
  }
  .options {
    padding: 20px 20px;
    flex-grow: 1;
    overflow-y: auto;
  }
  .take-screenshot {
    background: rgb(235, 240, 245);
    button {
      width: -webkit-fill-available;
      justify-content: center;
      padding: 12px 0;
      border: none;
      border-radius: 0;
      font-size: 1.1em;
      background: #0d6efd;
      color: rgb(250, 250, 250);
      font-family: 'SF Pro Display Medium';
      // margin: 18px 16px 16px;
    }
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
    width: -webkit-fill-available;
    // width: 80ch;
  }
  pre code {
    padding: 0;
    font-size: inherit;
  }

  .code-snippet-container {
    position: relative;
    margin: 50px 0;
  }

  // pre,
  // .code-snippet-container {
  //   width: 55em;
  // }

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
    button {
      font-size: 16px;
      padding: 0;
      justify-content: center;
      width: 25px;
      height: 25px;
      font-weight: 500;
      font-family: 'SF Pro Display Semibold';
      // border: none;
      background: white;
    }
  }
  .select-font-size-label {
    margin: auto 10px auto 0;
  }
  .font-size-display {
    text-align: center;
    margin: 0 5px;
    min-width: 36px;
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

  .windows {
    width: 100%;
    display: grid;
    img {
      margin: 2px 1px 0 auto;
    }
  }

  .show-os {
    display: flex;
    padding: 5px 0 15px;
    button {
      font-family: 'SF Pro Display Regular';
      font-size: 16px;
      padding: 5px 0;
      // border: none;
      background: white;
    }
    .active {
  background: #0d6efd;
  color: white;
    }
  }

  .os-btn {
    padding: 3px 8px;
    flex: 1;
    justify-content: center;
  }

  .mac-os-btn {
    border-left: none;
    border-right: none;
    border-radius: 0;
  }

  .none-os-btn {
    border-radius: 6px 0 0 6px;
  }
  .windows-btn {
    border-radius: 0 6px 6px 0;
  }

  // .active {
  //   background: #0d6efd;
  //   color: white;
  // }

  :global(.selector) {
    padding: 5px 0;
    :global(button) {
      width: 100%;
      font-family: 'SF Pro Display Regular';
      font-size: 16px;
      // border: none;
      background: white;
      padding: 8px 12px;
      margin-bottom: 5px;
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
    :global(label) {
      margin-bottom: 15px;
      :global(div) {
        border-radius: 0;
      }
    }
  }

  :global(button) {
    cursor: pointer;
  }

  .select-border-radius {
    padding: 8px 0;
    input {
      width: -webkit-fill-available;
    }
  }

  .select-font-size {
    padding: 5px 0 8px;
    .font-size-display {
      align-self: center;
    }
  }
  .show-language {
    padding: 10px 0;
  }
</style>
