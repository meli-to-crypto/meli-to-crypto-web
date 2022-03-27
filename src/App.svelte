<script>
  let coins = ["USDT", "DAI", "BTC", "ETH"];
</script>

<main>
  <h1 class="align-center">Mira los precios de Mercado Libre en Crypto</h1>
  <p class="paragraph-gray align-center">
    Ingresa el link de eso que te queres comprar y convertí su precio a la
    moneda que quieras.
  </p>
  <div class="form">
    <input
      placeholder="https://www.mercadolibre.com.ar/apple-iphone-..."
      type="text"
    />
    <select name="coins" id="coins">
      {#each coins as coin}
        <option value={coin}>{coin}</option>
      {/each}
    </select>
    <button>Pasar a Crypto</button>
  </div>
  <p id="mode-switcher" class="paragraph-gray align-center clickable">
    ¿Queres convertir el precio de algo que no esta en Mercado Libre? Usa la
    <span class="accent-text bold">Calculadora</span>
  </p>
</main>

<style>
  :root {
    --not-fully-black: #17191b;
    --paragraph-gray: #6c6969;
    --accent-blue: #1019f3;
    --input-border: #7c7e97;
  }

  main {
    font-family: "Inter", sans-serif;
    width: 1024px;
    max-width: 90vw;
    margin-left: auto;
    margin-right: auto;
  }

  .align-center {
    text-align: center;
  }

  .paragraph-gray {
    color: var(--paragraph-gray);
  }

  .accent-text {
	  color: var(--accent-blue)
  }

  .bold {
	font-weight: bold;
  }

  .clickable {
	cursor: pointer;
  }

  h1 {
    color: var(--not-fully-black);
    font-size: 72px;
  }

  button {
    background-color: var(--accent-blue);
    color: white;
    border: 0px;
    border-radius: 8px;
    padding: 8px 16px;
  }

  .form {
    display: flex;
    justify-content: center;
    margin-top: 2em;
  }

  input,
  select {
    border: 1px solid var(--input-border);
    border-radius: 8px;
    padding: 8px 16px;
    margin-right: 16px;
  }

  #mode-switcher {
	margin-top: 4em;
  }
</style>
