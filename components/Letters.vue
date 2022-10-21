<template>
  <div class="wrap">
    <div class="letters">
      <nuxt-link to="learning/a" class="heading letter">
        a
      </nuxt-link>
      <nuxt-link to="learning/b" class="heading letter">
        b
      </nuxt-link>
      <nuxt-link to="learning/c" class="heading letter">
        c
      </nuxt-link>
      <nuxt-link to="learning/d" class="heading letter">
        d
      </nuxt-link>
      <nuxt-link to="learning/e" class="heading letter">
        e
      </nuxt-link>
      <nuxt-link to="learning/f" class="heading letter">
        f
      </nuxt-link>
      <nuxt-link to="learning/g" class="heading letter">
        g
      </nuxt-link>
      <nuxt-link to="learning/h" class="heading letter">
        h
      </nuxt-link>
      <nuxt-link to="learning/i" class="heading letter">
        i
      </nuxt-link>
      <nuxt-link to="learning/k" class="heading letter">
        k
      </nuxt-link>
      <nuxt-link to="learning/l" class="heading letter">
        l
      </nuxt-link>
      <nuxt-link to="learning/m" class="heading letter">
        m
      </nuxt-link>
      <nuxt-link to="learning/n" class="heading letter">
        n
      </nuxt-link>
      <nuxt-link to="learning/o" class="heading letter">
        o
      </nuxt-link>
      <nuxt-link to="learning/p" class="heading letter">
        p
      </nuxt-link>
      <nuxt-link to="learning/q" class="heading letter">
        q
      </nuxt-link>
      <nuxt-link to="learning/r" class="heading letter">
        r
      </nuxt-link>
      <nuxt-link to="learning/s" class="heading letter">
        s
      </nuxt-link>
      <nuxt-link to="learning/t" class="heading letter">
        t
      </nuxt-link>
      <nuxt-link to="learning/u" class="heading letter">
        u
      </nuxt-link>
      <nuxt-link to="learning/v" class="heading letter">
        v
      </nuxt-link>
      <nuxt-link to="learning/w" class="heading letter">
        w
      </nuxt-link>
      <nuxt-link to="learning/x" class="heading letter">
        x
      </nuxt-link>
      <nuxt-link to="learning/y" class="heading letter">
        y
      </nuxt-link>
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
