<script>
  let coins = ["USDT", "DAI", "BTC", "ETH"];

  // Guardo aca el item que quiere convertir el usuario y la moneda que eligio.
  let itemUrl = "";
  let selectedCoin;
  let itemId;

  // Una vez llamadas las apis de meli y belo, guardo los datos en estas variables.
  let itemPriceInPesos;
  let itemPriceInCrypto;
  let exhangeRate;

  // mientras llamo a las apis se muestra un loading.
  let loading = false;
  // si la url que puso el usuario no coincide con meli ponemos un error.
  let invalidUrl = false;

  // On click of the button, use item url to fetch its price from mercadolibre
  // then use the belo api to fetch the exchangerate
  // then calculate the price in the selected crypto
  function convertor() {
    console.log(itemUrl);
    console.log(selectedCoin);
    if (isValidUrl) {
      // we start loading animation
      loading = true;
      invalidUrl = false;
      // we extract the itemId from the itemUrl
      let itemId = itemIdExtractor();
      // we fetch the price from mercadolibre
      console.log(itemId);
      fetch(`https://api.mercadolibre.com/items?ids=MLA${itemId}`)
        .then((res) => res.json())
        .then((res) => {
          itemPriceInPesos = res[0].body.price;
          console.log("El precio es: ", itemPriceInPesos);
          exhangeRate = fetch("https://beta.belo.app/public/price")
            .then((res) => res.json())
            .then((res) => {
              // filter res by object containing pairCode == 'selectedCoin/ARS'
              exhangeRate = res.filter(
                (item) => item.pairCode === `${selectedCoin}/ARS`
              )[0].bid;
              console.log(res);
              console.log(exhangeRate);
              // we calculate the price in the selected crypto
              itemPriceInCrypto = itemPriceInPesos / exhangeRate;
              console.log(
                `El precio en crypto es ${itemPriceInPesos} / ${exhangeRate} = ${itemPriceInCrypto}`
              );
              // we stop loading animation
              loading = false;
              alert('El precio en pesos es: ' + itemPriceInPesos + ' y el precio en crypto es: ' + itemPriceInCrypto.toFixed( 2 ) + ' '+ selectedCoin);
            });
        });
    } else {
      invalidUrl = true;
    }
  }

  // check if itemUrl is a valid url
  function isValidUrl() {
    // regex to check if itemUrl is a valid url
    const regex =
      /^(?:http(s)?:\/\/)?[\w.-]+(?:\.[\w\.-]+)+[\w\-\._~:/?#[\]@!\$&'\(\)\*\+,;=.]+$/gm;
    console.log(regex.exec(itemUrl)[0]);
    return regex.test(itemUrl);
  }

  function itemIdExtractor() {
    // regex to extract the item id from the url
    const regex = /[0-9]{8,}/gm;
    itemId = regex.exec(itemUrl)[0];
    console.log("El item id es: ", itemId);
    return itemId;
  }
</script>

<main>
  <h1 class="align-center">Mira los precios de Mercado Libre en Crypto</h1>
  <p class="paragraph-gray align-center">
    Ingresa el link de eso que te queres comprar y convertí su precio a la
    moneda que quieras.
  </p>
  <div class="form">
    <input
      bind:value={itemUrl}
      placeholder="https://www.mercadolibre.com.ar/apple-iphone-..."
      type="text"
    />
    <select bind:value={selectedCoin} name="coins" id="coins">
      {#each coins as coin}
        <option value={coin}>{coin}</option>
      {/each}
    </select>
    <button class="clickable" on:click={convertor}>Pasar a Crypto</button>
  </div>
  <p id="mode-switcher" class="paragraph-gray align-center clickable">
    ¿Queres convertir el precio de algo que no esta en Mercado Libre? Usa la
    <span class="accent-text bold">Calculadora</span>
  </p>
  <a
    id="chrome-webstore"
    href="https://chrome.google.com/webstore/detail/meli-a-usdt/pabjndhejioccdbodimjhmgbjhhgphgl"
  >
    <img
      src="https://wd.imgix.net/image/BrQidfK9jaQyIHwdw91aVpkPiib2/LclHxMxqoswLNRcUW3m5.png?auto=format&w=260"
      alt="available in the chrome webstore"
    />
  </a>
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
    padding-top: 2em;
  }

  .align-center {
    text-align: center;
  }

  .paragraph-gray {
    color: var(--paragraph-gray);
  }

  .accent-text {
    color: var(--accent-blue);
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
    margin-top: 4em;
  }

  input,
  select {
    border: 1px solid var(--input-border);
    border-radius: 8px;
    margin-right: 16px;
  }

  input,
  select,
  button {
    padding: 8px 16px;
  }

  input {
    width: 400px;
  }

  p {
    font-size: 24px;
    max-width: 830px;
    margin-left: auto;
    margin-right: auto;
  }

  #mode-switcher {
    margin-top: 4em;
    font-size: 20px;
  }

  #chrome-webstore {
    position: absolute;
    top: 16px;
    right: 16px;
  }

  #chrome-webstore img {
    max-width: 200px;
  }
</style>
