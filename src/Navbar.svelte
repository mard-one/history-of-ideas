<script>
  import { Link } from "svelte-routing";
</script>

<nav>
  <div class="logo">
    <Link to="/">History of ideas</Link>
  </div>
  <ul class="menu">
    <li>
      <Link to="timeline">Timeline</Link>
    </li>
    <li>
      <Link to="ideas">List</Link>
    </li>
  </ul>
  <div class="contribute">
    <Link to="/contribute">Contribute</Link>
  </div>
</nav>

<style>
  nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px;
  }
  .logo {
    width: 200px;
    padding-right: 32px;
    font-family: Palatino, "Palatino Linotype", "Palatino LT STD",
      "Book Antiqua", Georgia, serif;
    font-size: 24px;
    font-style: bold;
    font-variant: normal;
    font-weight: 700;
    line-height: 26.4px;
  }
  :global(.logo a) {
    text-decoration: none !important;
  }
  .menu {
    flex-grow: 1;
    max-width: 1080px;
    display: flex;
  }
  .menu li {
    padding-right: 16px;
    list-style-type: none;
  }
  .contribute {
    width: 100px;
    padding-left: 32px;
    text-align: right;
  }
  :global(a[aria-current]) {
    text-decoration: underline;
  }
  ul {
    margin: 0;
    padding: 0;
  }
</style>
