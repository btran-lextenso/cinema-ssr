<script>
  let showOnPx = 400;
  let hidden = true;

  function goTop() {
    document.body.scrollIntoView();
  }

  function scrollContainer() {
    return document.documentElement || document.body;
  }

  function handleOnScroll() {
    if (!scrollContainer()) {
      return;
    }

    if (scrollContainer().scrollTop > showOnPx) {
      hidden = false;
    } else {
      hidden = true;
    }
  }
</script>

<style>
  .back-to-top {
    opacity: 1;
    transition: opacity 2s, visibility 2s;
    position: fixed;
    z-index: 99;
    right: 50%;
    user-select: none;
    bottom: 0;
    color: #fff;
    background-color: black;
    padding: 10px;
    transform: translate(30px, -70px);
  }
  .back-to-top:hover{
    cursor: pointer
  }

  .back-to-top.hidden {
    opacity: 0;
    visibility: hidden;
  }
  .arrow-up {
    width: 32px;
    height: 16px;
    background-color: #fff;
    clip-path: polygon(50% 0, 0 100%, 100% 100%);
    display: block;
    margin: 0 auto;
  }
</style>

<svelte:window on:scroll={handleOnScroll} />

<div class="back-to-top" on:click={goTop} class:hidden>
  <span class="arrow-up"></span>
  Retour
</div>
