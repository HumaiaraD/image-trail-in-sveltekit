<script>
  import { onMount } from "svelte";
  import gsap from "gsap";

  let container;
  let imagesIndex = 1;
  let animationTimeout = null;
  let isAnimating = false;

  function addNewItem(x, y) {
    const newItem = document.createElement("div");
    newItem.className = "item";
    newItem.style.position = "absolute";
    newItem.style.left = `${x - 75}px`;
    newItem.style.top = `${y - 75}px`;
    newItem.className = `absolute w-[150px] h-[150px] pointer-events-none`;


    const img = document.createElement("img");
    img.src = `/images/img${imagesIndex}.jpg`;
    img.className = "w-50 h-60  object-cover";

    newItem.appendChild(img);
    imagesIndex = (imagesIndex % 15) + 1;

    container.appendChild(newItem);
    manageItemLimit();
  }

  function manageItemLimit() {
    while (container.children.length > 20) {
      container.removeChild(container.firstChild);
    }
  }

  function startAnimating() {
    if (isAnimating || container.children.length === 0) return;
    isAnimating = true;

    gsap.to(".item", {
      y: 1000,
      scale: 0.5,
      opacity: 0,
      duration: 0.5,
      stagger: 0.025,
      onComplete: function () {
        this.targets().forEach((item) => {
          item.remove();
        });
        isAnimating = false;
      },
    });
  }

  onMount(() => {
    container.addEventListener("mousemove", (event) => {
      clearTimeout(animationTimeout);
      addNewItem(event.pageX, event.pageY);
      animationTimeout = setTimeout(startAnimating, 100);
    });
  });
</script>



<h1 class="text-3xl text-center">Move your mouse around</h1>
<div id="items" class="relative w-[100vw] h-[100vh] overflow-hidden" bind:this={container}></div>
