<script lang="ts">
  import cat from "./assets/cat.png";
  import milk from "./assets/milk.webp";

  let count: number = 0;
  let multi: number = 1;
  let price: number = 10;

  let myInput: HTMLDivElement;
  let catClick: HTMLDivElement;
  const load = () => {
    const data = localStorage.getItem("cat_jumper");

    if (data === null) {
      return;
    }

    try {
      return JSON.parse(data);
    } catch (e) {
      localStorage.removeItem("cat_jumper");
      return;
    }
  };

  const loadData = load();
  if (loadData) {
    count = loadData.count;
    multi = loadData.multi;
    price = loadData.price;
  }
  const increment = () => {
    count += multi;

    class Coint {
      constructor() {
        let div = document.createElement("div");
        div.textContent = `+ ${multi}`;
        div.classList.add("coin");

        myInput.appendChild(div);
        setTimeout(() => {
          div.remove();
        }, 1000);
      }
    }
    new Coint();
    save();
  };

  const update = () => {
    if (count < price) {
      return;
    }
    count = count - price;
    multi += 1;
    price = Math.round(price * 1.5);
    save();
  };

  const save = () => {
    const data = {
      count,
      multi,
      price,
    };

    localStorage.setItem("cat_jumper", JSON.stringify(data));
  };
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<main class="main">
  <div class="card">
    <h2 class="money">Money:<br /> {count}$</h2>
  </div>
  <div>
    <div bind:this={myInput} id="animat"></div>
    <!-- svelte-ignore a11y-missing-attribute -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
    <img bind:this={catClick} on:click={increment} src={cat} class="logo" />
  </div>

  <!-- svelte-ignore a11y-missing-attribute -->
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
  <div class="tool">
    <div class="flex">
      <div class="milk_count">{multi}</div>
      <img on:click={update} class="milk" src={milk} />
    </div>
    <div class="price">{price}$</div>
  </div>
</main>

<style>
  .flex {
    display: flex;
    gap: 8px;
    align-items: center;
    justify-content: space-between;
  }

  .main {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .tool {
    width: 100%;
    display: flex;
    flex-direction: column;
  }
  #animat {
    position: relative;
  }
  .milk {
    height: 88px;
    transition: transform 200ms;
  }
  .milk:hover {
    filter: drop-shadow(0 0 2em #22ff00aa);
    transform: scale(1.05);
  }
  .milk:active {
    filter: drop-shadow(0 0 2em #22ff00aa);
    transform: scale(0.95);
  }
  .logo {
    height: 192px;
    padding: 1.5em;
    padding-top: 70px;
    will-change: filter;
    transition: filter 300ms;
    transition: transform 200ms;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
    transform: scale(1.05);
  }

  .logo:active {
    filter: drop-shadow(0 0 2em #ff3e00aa);
    transform: scale(0.95);
  }

  .milk_count {
    font-weight: 800;
    font-size: 72px;
  }

  .price {
    font-weight: 800;
    font-size: 36px;
    color: green;
    text-shadow: rgb(217, 255, 0) 0px 0px 30px;
  }
  .money {
    font-weight: 800;
    font-size: 46px;
    color: green;
    text-shadow: rgb(217, 255, 0) 0px 0px 30px;
  }
</style>
