<script>
	export let segment;
</script>
<a href= "/">
   <div class="col-md-6">
	<center>
	    <img src="MFFLogo.png" alt="" width = 250px height = 60px  align="right" vertical-align ="middle"/>
	</center>
</div>

</a>


<style>
	nav {
		border-bottom: 1px solid rgba(105, 161, 226, 0.993);
		font-weight: 300;
		padding: 0 1em;
	}

	ul {
		margin: 0;
		padding: 0;
	}

	/* clearfix */
	ul::after {
		content: '';
		display: block;
		clear: both;
	}

	li {
		display: block;
		float: left;
	}

	[aria-current] {
		position: relative;
		display: inline-block;
	}

	[aria-current]::after {
		position: absolute;
		content: '';
		width: calc(100% - 1em);
		height: 2px;
		background-color: rgb(0, 102, 255);
		display: block;
		bottom: -1px;
	}

	a {
		text-decoration: none;
		padding: 1em 0.5em;
		display: block;
	}
</style>

<nav>
	<ul>
		<li><a aria-current='{segment === undefined ? "page" : undefined}' href='.'>home</a></li>
		<li><a aria-current='{segment === "mapVisual" ? "page" : undefined}' href='mapVisual'>map</a></li>
		<li><a aria-current='{segment === "about" ? "page" : undefined}' href='about'>about</a></li>
	</ul>
</nav>
