<script>
  import ChordDiagram from './components/ChordDiagram.svelte';

  let chordInput = '';
  let chordVariations = [];

  async function handleSubmit() {
    const chords = chordInput.split(',').map(chord => chord.trim());
    try {
      const response = await fetch('/variations', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(chords),
      });
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      chordVariations = await response.json();
    } catch (error) {
      console.error('Error:', error);
      chordVariations = [];
    }
  }
</script>

<main class="container mx-auto px-4 py-8">
  <h1 class="text-5xl font-bold mb-6">Variations</h1>
  <div class="input-container mb-6">
    <input
      type="text"
      bind:value={chordInput}
      placeholder="Enter chords separated by commas (e.g., A, D, G)"
      class="w-full p-2 border border-gray-300 rounded"
    />
    <button on:click={handleSubmit} class="mt-2 bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">
      Get Optimal Transitions
    </button>
  </div>
  {#if chordVariations.length > 0}
    <h2 class="text-2xl font-semibold mb-4">Variations:</h2>
    <div class="grid grid-cols-2 sm:grid-cols-4 md:grid-cols-5 lg:grid-cols-5 gap-4">
      {#each chordVariations as chord (chord.name)}
        <div>
          <ChordDiagram {chord} />
        </div>
      {/each}
    </div>
  {/if}
</main>

<style>
  :global(body) {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
  }
</style>
