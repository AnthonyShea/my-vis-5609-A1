<script>
    // state variables
    let maxClick = 2; // default value
    let cnt = maxClick;
  
    function onClick() {
      if (cnt > 0) {
        cnt -= 1;
      }
    }
  
    function onMaxChange() {
      // reset counter when user changes the maxClick value
      cnt = maxClick;
    }
  </script>
  
  <h1>Anthony's VIS Site</h1>
  <img src="/FullSizeRender.jpg" width="200px" alt="Profile photo" />

  
  <div>
    You can click up to
    <select bind:value={maxClick} on:change={onMaxChange}>
      {#each [2, 4, 6] as optionNum}
        <option value={optionNum}>{optionNum}</option>
      {/each}
    </select>
    times
  </div>
  
  <button on:click={onClick}>Click Me</button>
  
  {#if cnt > 0}
    <p id="info">Remaining Number of Clicks: {cnt}</p>
  {:else}
    <p>No more clicks allowed</p>
  {/if}
  
  <style>
    body {
      font-family: Arial, Helvetica, sans-serif;
    }
    button {
      background-color: #44aa66;
      color: white;
      font-size: xx-large;
      padding: 10px 20px;
      border: none;
      cursor: pointer;
      border-radius: 5px;
    }
  </style>
  