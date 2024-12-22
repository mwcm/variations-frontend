<script>
  export let chord;

  $: fretPositions = chord.positions.map(pos => pos === 'x' ? pos : parseInt(pos));
  $: maxFret = Math.max(...fretPositions.filter(pos => typeof pos === 'number'));
  $: minFret = Math.min(...fretPositions.filter(pos => typeof pos === 'number' && pos > 0));
  $: fretOffset = minFret > 3 ? minFret - 1 : 0;

  function getFretLabel(index) {
    return index + fretOffset;
  }
</script>

<div class="chord-diagram">
  <h3 class="chord-name">{chord.name}</h3>
  <div class="fretboard">
    <div class="open-mute-row">
      {#each chord.positions as position}
        <div class="marker">
          {#if position === 'x'}
            <span class="muted">✕</span>
          {:else if position === '0'}
            <div class="open-circle"></div>
          {/if}
        </div>
      {/each}
    </div>
    <div class="grid-container">
      <div class="fret-numbers">
        {#each Array(4).fill() as _, fretIndex}
          <div class="fret-number">{getFretLabel(fretIndex + 1)}</div>
        {/each}
      </div>
      <div class="grid">
        <div class="strings">
          {#each Array(6).fill() as _, i}
            <div class="string"></div>
          {/each}
        </div>
        <div class="frets">
          {#each Array(4).fill() as _, fretIndex}
            <div class="fret">
              {#each chord.positions as position, stringIndex}
                <div class="cell">
                  {#if parseInt(position) === getFretLabel(fretIndex + 1)}
                    <div class="finger-dot">
                      {chord.fingerings[stringIndex]}
                    </div>
                  {/if}
                </div>
              {/each}
            </div>
          {/each}
        </div>
        <div class="fretboard-edge left"></div>
        <div class="fretboard-edge right"></div>
      </div>
    </div>
  </div>
</div>

<style>
  .chord-diagram {
    width: 120px;
    font-family: Arial, sans-serif;
  }

  .chord-name {
    font-size: 1.2rem;
    text-align: center;
    margin: 0 0 5px 0;
    font-weight: normal;
  }

  .fretboard {
    display: flex;
    flex-direction: column;
  }

  .open-mute-row {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    height: 16px;
    margin-bottom: 2px;
    margin-left: 20px;
  }

  .marker {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .open-circle {
    width: 8px;
    height: 8px;
    border: 1px solid black;
    border-radius: 50%;
  }

  .muted {
    font-size: 12px;
  }

  .grid-container {
    display: flex;
    gap: 5px;
  }

  .fret-numbers {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    padding: 1px 0;
  }

  .fret-number {
    font-size: 10px;
    height: 24px;
    display: flex;
    align-items: center;
  }

  .grid {
    position: relative;
    flex-grow: 1;
    border-top: 2px solid black;
  }

  .strings {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    display: grid;
    grid-template-columns: repeat(6, 1fr);
  }

  .string {
    width: 1px;
    height: 100%;
    background: black;
    margin: 0 auto;
  }

  .frets {
    display: flex;
    flex-direction: column;
  }

  .fret {
    height: 24px;
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    border-bottom: 1px solid black;
  }

  .cell {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .finger-dot {
    width: 16px;
    height: 16px;
    background: black;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: white;
    font-size: 10px;
  }

  .fretboard-edge {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 2px;
    background-color: black;
  }

  .fretboard-edge.left {
    left: -1px;
  }

  .fretboard-edge.right {
    right: -1px;
  }
</style>

