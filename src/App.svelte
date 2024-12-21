<script>
  let chordInput = '';
  let apiResponse = '';

  async function handleSubmit() {
    const chords = chordInput.split(',').map(chord => chord.trim());
    try {
      const response = await fetch('/variations', {
        method: 'POST',
        headers: {
          'Content-Type':  'application/json',
        },
        body: JSON.stringify(chords),
      })

      if (!response.ok) {
        throw new Error(`HTTP ERROR status: ${response.status}`);
      }
      const data = await response.json();
      apiResponse = JSON.stringify(data, null, 2);
    } catch (error) {
      console.error('error:', error)
      apiResponse = `Error: ${error.message}`; 
    }
  }
</script>



<main>
  <h1>Variations</h1>
  <div class='container'>
    <input
      type="text"
      bind:value={chordInput}
      placeholder="Enter Chords separated by commas"
      />
    <button on:click={handleSubmit}>Get Variations</button>
  </div>
  {#if apiResponse}
    <h2>API Response:</h2>
    <pre>{apiResponse}</pre>
  {/if}
</main>


<style>
  main {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    font-family: Arial, sans-serif;
  }

  h1, h2 {
    color: #333;
  }

  .container {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
  }

  input {
    flex-grow: 1;
    padding: 10px
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  button {
    background-color: #4CAF50;
    border: none;
    color: white;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    cursor: pointer;
    border-radius: 4px;
  }

  pre {
    padding: 15px;
    border-radius: 4px;
    white-space: pre-wrap;
    word-wrap: break-word;
  }

</style>
