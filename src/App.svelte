<script>
	import Red from "./red.svelte";

let page = document.location.hash;

window.onpopstate = function(event) {
        page = document.location.hash;
    };

</script>

<main>
    <div class="row">
        <div class="column">
            <h1>Menu</h1>
            <a href="#home">Home</a>
            <a href="#red">Red Page</a>
            <a href="#green">Green Page</a>
        </div>
        <div class="column">
            {#if page==="#home"}
              Home Page
            {:else if page === "#red"}
              <Red />
            {:else if page === "#green"}
              Green page
            {:else}
              404: Page not Found
            {/if}
        </div>
    </div>
</main>

<style>
	.row {
	  display: flex;
	  flex-direction: row;
	  flex-wrap: wrap;
	  width: 100%;
	}
	
	.column {
	  display: flex;
	  flex-direction: column;
	  flex-basis: 100%;
	  flex: 1;
	}
</style>