<script>
    export let index;

    let isVisible = true;
    let imgurl = "/src/assets/bgimage.PNG";
    // let testurls = {0: "/src/assets/test.PNG", 
    //                 1: "/src/assets/economic.PNG", 
    //                 2: "/src/assets/people.PNG", 3: "/src/assets/deaths.PNG"};

    $: if (index >= 1) {
        isVisible = true;
    } else {
        isVisible = false;
    }
</script>

<main class:visible={isVisible}>
    <!-- <img src={testurls[index]} alt="background image" class="center"/> -->
    <img src={imgurl} alt="background image" class="center"/>
</main>

<style>
    .center {
        display: block;
        margin-left: auto;
        margin-right: auto;
        width: 80%;
        height: 90%;
        }
    main {
    /* width: 100%;
    height: 100vh;
    position: absolute; */
    opacity: 0;
    visibility: hidden;
  }

  main.visible {
    opacity: 1;
    visibility: visible;
  }

</style>