<div class="card">
  <slot/>
</div>

<style lang="scss">

  @import "./src/scss/config";

  $horizontalSpacingCards: 0.2rem !default;

  .card {

    margin: 0 ($horizontalSpacingCards * 0.5) 0.5rem;
    padding: 0.5rem;
    box-shadow: 0 0 $horizontalSpacingCards rgba(0, 0, 0, 0.3);

    @media screen and (max-width: 300px) {
      padding: 0.5rem 0.25rem;
    }

    @media (prefers-color-scheme: dark) {
      box-shadow: 0 0 $horizontalSpacingCards #000;
      background-color: #2a2929;
    }

    &:hover {
      background-color: #fafafa;
      @media (prefers-color-scheme: dark) {
        background-color: #302f2f;
      }
    }

  }

</style>
