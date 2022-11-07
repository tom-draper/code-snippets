<script lang="ts">
  let hi: string = "hi"
  let borderRadius = 10;
  let terminalBackground = '#25323a'
  let background = '#bfe4f7'
  let fontSize = 16;
</script>

<div>
  <div class="page-content">
    <div class="viewer" style="background: {background};">
      <div class="code-snippet-container" style="border-radius: {borderRadius}px;">
        <textarea  id="codeSnippet" name="code-snippet" rows="30" cols="80" style="background: {terminalBackground}; border-radius: {borderRadius}px; font-size: {fontSize}px;"/>
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
    color: white;
    width: 100%;
    height: 100%;
    background: rgb(176, 176, 176);
    padding: 20px;
    border: none;
    resize: none;
  }
</style>
