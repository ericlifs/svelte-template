<script>
  import Button from '@sapper-template/ui-kit/Button.svelte';
  import Nav from '@components/Nav.svelte';

  export let segment
</script>

<style lang="scss" global>
  @import './styles/global.scss';

  main {
    position: relative;
    max-width: 56em;
    background-color: white;
    padding: 2em;
    margin: 0 auto;
    box-sizing: border-box;
  }
</style>

<Nav {segment} />

<main>
  <Button buttonText="Test"/>
  <slot />
</main>
