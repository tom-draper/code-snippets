<script lang="ts">
  import {onMount} from 'svelte';
  import hljs from 'highlight.js';
  import 'highlight.js/styles/atom-one-dark.css';

  // hljs.highlightAll();

  function renderCode() {
    let snippet = document.getElementById("code");
    if (snippet != null) {
      snippet.innerHTML = code
      hljs.highlightElement(snippet);
    }
    console.log(code)
    console.log(snippet)
  }

  let borderRadius = 10;
  let terminalBackground = '#25323a'
  let background = '#bfe4f7'
  let fontSize = 16;
  let padding = 15;
  let code = "from dataclass import dataclass\nimport numpy as np"
  $: code && renderCode();
  $: code == "" && renderCode()
  let language = "python"
  let font = "Fira Code"
  onMount(() => {
    renderCode()
  })
</script>

<div>
  <div class="page-content">
    <div class="viewer" style="background: {background};">
      <div class="code-snippet-container" style="border-radius: {borderRadius}px;">
        <pre><code id="code" class="language-{language}" style="font-family: {font} !important; margin: {padding}px">{code}</code></pre>
        <textarea bind:value={code} id="codeSnippet" name="code-snippet" style="background: {terminalBackground}; border-radius: {borderRadius}px; font-size: {fontSize}px; font-family: {font} !important; padding: {padding}px;"/>
      </div>
    </div>
    <div class="controller">Controller</div>
  </div>
</div>

<style scoped>
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
    width: 350px;
  }
  #codeSnippet {
    color: transparent;
    background: rgb(176, 176, 176);
    border: none;
    resize: none;
    caret-color: white;
    width: 710px;
    height: 500px;
  }

  :global(pre) {
    position: absolute !important;
  }

  pre {
    pointer-events: none;
    margin: 0;
  }
  pre code {
    padding: 0;
    width: 700px;
  }

  #code {
    background: transparent !important;
    width: 700px;
    white-space: pre-wrap;
  }

</style>
