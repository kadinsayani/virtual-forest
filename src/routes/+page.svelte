<div class="welcome-container">
  <div class="welcome-label">
    <h1>Welcome to Nature Explorer: Your Virtual Forest Adventure!</h1>
  </div>

  <p class="description">
    🌳 Embark on an exciting journey into the heart of nature with Nature
    Explorer, where young explorers like you can uncover the wonders of the
    forest right from your screen! 🌿
  </p>

  <p class="description-button">
    🌳 Discover the Wildlife: Click on animals, trees, birds, and more to find
    and interact with them. Each click reveals fascinating facts and insights
    about the species that call the forest home.
  </p>

  <p class="description-button">
    🦌 Interact with the Ecosystem: Use the sliders to adjust the population
    counts of various species to learn how they affect the ecosystem and expand
    your knowledge about the incredible world of nature.
  </p>
  <p class="description-button">
    🐾 Adventure Awaits: There's always something new to uncover in Nature
    Explorer. Are you ready to become a true nature explorer and protector of
    the forest? Join us on this educational and entertaining adventure!
  </p>

  <a href="/forest" class="start-button"><b>Start Exploring!🦋🐦</b></a>
</div>

<style>
  :global(:root) {
    font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
      Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
      sans-serif;
    color: white;
    background-image: linear-gradient(#001e00, green);
  }

  a {
    text-decoration: none;
    color: white;
  }

  .welcome-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    text-align: center;
  }

  .welcome-label {
    font-size: larger;
  }

  .description {
    max-width: 60%;
    margin: 20px 0;
    font-size: 1.2rem;
  }

  .description-button {
    display: inline-block;
    width: 60%;
    padding: 10px 20px;
    background-color: lightgrey;
    border: none;
    border-radius: 17px;
    color: green;
    font-size: 1.2rem;
    text-align: center;
  }
  .start-button {
    display: inline-block;
    padding: 15px 20px;
    background-color: white;
    margin-top: 20px;
    border: none;
    border-radius: 30px;
    color: green;
    font-size: 1.5rem;
    text-align: center;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .start-button:hover {
    background-color: #45a049;
    color: black;
  }
</style>
