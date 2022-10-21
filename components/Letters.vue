<template>
  <div class="wrap">
    <div class="letters">
      <a href="learning/a" class="heading letter">a</a>
      <a href="learning/b" class="heading letter">b</a>
      <a href="learning/c" class="heading letter">c</a>
      <a href="learning/d" class="heading letter">d</a>
      <a href="learning/e" class="heading letter">e</a>
      <a href="learning/f" class="heading letter">f</a>
      <a href="learning/g" class="heading letter">g</a>
      <a href="learning/h" class="heading letter">h</a>
      <a href="learning/i" class="heading letter">i</a>
      <a href="learning/k" class="heading letter">k</a>
      <a href="learning/l" class="heading letter">l</a>
      <a href="learning/m" class="heading letter">m</a>
      <a href="learning/n" class="heading letter">n</a>
      <a href="learning/o" class="heading letter">o</a>
      <a href="learning/p" class="heading letter">p</a>
      <a href="learning/q" class="heading letter">q</a>
      <a href="learning/r" class="heading letter">r</a>
      <a href="learning/s" class="heading letter">s</a>
      <a href="learning/t" class="heading letter">t</a>
      <a href="learning/u" class="heading letter">u</a>
      <a href="learning/v" class="heading letter">v</a>
      <a href="learning/w" class="heading letter">w</a>
      <a href="learning/x" class="heading letter">x</a>
      <a href="learning/y" class="heading letter">y</a>
    </div>
  </div>
</template>

<style lang="scss">
.wrap{
    .letters{
        margin: 0 7em 0;
        display: grid;
        gap: .5em;
        grid-template-columns: repeat(6, 1fr);
        grid-template-rows: repeat(4, 1fr);

        .letter{
            padding: .5em;
            display: grid;
            place-items: center;
            color: $clr-light;
            border: 1px solid #ffffff45;
            border-radius: 5px;
            transition: .25s;
            font-family: $ff-heading;

            &:hover{
                color: $clr-blue;
                background-color: $clr-yellow;
            }
        }

    }
    .caption{
        margin: 2em 0 0;
        width: 100%;
        text-align: center;
    }
}
</style>
