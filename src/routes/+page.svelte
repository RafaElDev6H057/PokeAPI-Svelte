<script>
	let pokemon;
	let seHaBuscado = false;
	const typeColors = {
		normal: 'var(--type-normal)',
		fire: 'var(--type-fire)',
		water: 'var(--type-water)',
		grass: 'var(--type-grass)',
		electric: 'var(--type-electric)',
		ice: 'var(--type-ice)',
		fighting: 'var(--type-fighting)',
		poison: 'var(--type-poison)',
		ground: 'var(--type-ground)',
		flying: 'var(--type-flying)',
		psychic: 'var(--type-psychic)',
		bug: 'var(--type-bug)',
		rock: 'var(--type-rock)',
		ghost: 'var(--type-ghost)',
		dark: 'var(--type-dark)',
		dragon: 'var(--type-dragon)',
		steel: 'var(--type-steel)',
		fairy: 'var(--type-fairy)'
	};

	let isShowStats = false;
	function mostrarStats() {
		if (isShowStats === false) {
			isShowStats = true;
		} else {
			isShowStats = false;
		}
	}

	async function getPokemon(pokemon) {
		const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${pokemon}`);
		const data = await response.json();
		const { id, name, height, weight, sprites, types, stats } = data;
		const { other } = sprites;
		const officialArtwork = other['official-artwork'].front_default;
		const tipos = types.map((typeInfo) => typeInfo.type.name);
		const estadisticas = stats.map((statInfo) => statInfo.base_stat);
		return {
			clave: id,
			nombre: name,
			peso: weight,
			altura: height,
			imagen: officialArtwork,
			tipos: tipos,
			estadisticas: estadisticas
		};
	}

	// let pokemon = getPokemon();

	function handleClick() {
		const pokemonBuscado = document.getElementById('idPoke').value;
		const pokemonMinus = pokemonBuscado.toLowerCase();
		pokemon = getPokemon(pokemonMinus);
		seHaBuscado = true;
	}
</script>

<h1 class="text-3xl text-blue-500 font-bold text-center my-10">Busca a tu Pokémon!</h1>

<div class="max-w-[500px] w-[90%] mx-auto bg-blue-400 p-4 rounded-xl" id="errorPokemon">
	<label class="block text-white font-bold text-xl mb-2" for="idPoke"
		>Ingresa el nombre del pokémon:</label
	>
	<input
		class="bg-orange-300 block w-full mb-4 rounded-sm py-2 px-4 font-bold focus-visible:outline-none placeholder:text-slate-500"
		type="text"
		name="idPoke"
		id="idPoke"
		placeholder="Escribe el nombre del pokémon"
	/>
	<button
		class="bg-blue-600 text-white font-bold text-2xl mb-10 py-2 px-4 rounded hover:bg-blue-500 duration-200"
		on:click={handleClick}>Buscar</button
	>

	{#if seHaBuscado}
		{#await pokemon}
			<p class="text-xl font-bold">Buscando...</p>
		{:then data}
			<div class="bg-blue-200 p-5 text-center relative isolate uppercase overflow-hidden">
				<p
					class="absolute top-1/2 -translate-y-1/2 left-1/2 -translate-x-1/2 text-9xl font-bold -z-10 opacity-30 text-pretty"
				>
					#{data.clave.toString().padStart(3, '0')}
				</p>

				<div class=" hidden sm:grid sm:grid-cols-2 sm:gap-x-8">
					<div class="flex flex-col items-start gap-y-1">
						<p class="bg-[#69dc12] rounded-lg px-4 py-2 text-[12px] font-bold w-full">
							Vida: {data.estadisticas[0]}
						</p>
						<p class="bg-[#efcc18] rounded-lg px-4 py-2 text-[12px] font-bold w-full">
							Ataque: {data.estadisticas[1]}
						</p>
						<p class="bg-[#e86412] rounded-lg px-4 py-2 text-[12px] font-bold w-full">
							Defensa: {data.estadisticas[2]}
						</p>
					</div>

					<div class="flex flex-col items-end gap-y-1">
						<p class="bg-[#14c3f1] rounded-lg px-4 py-2 text-[12px] font-bold w-full">
							Ataque Especial: {data.estadisticas[3]}
						</p>
						<p class="bg-[#4a6adf] rounded-lg px-4 py-2 text-[12px] font-bold w-full">
							Defensa Especial: {data.estadisticas[4]}
						</p>
						<p class="bg-[#d51dad] rounded-lg px-4 py-2 text-[12px] font-bold w-full">
							Velocidad: {data.estadisticas[5]}
						</p>
					</div>
				</div>

				<div class="flex justify-center">
					<img class="w-40 h-40" src={data.imagen} alt={data.nombre} />
				</div>
				<div class="flex justify-center items-center gap-x-4 mb-4 flex-wrap">
					<p class="font-bold text-sm bg-orange-300 rounded-2xl py-1 px-2">
						#{data.clave.toString().padStart(3, '0')}
					</p>
					<h2 class="text-3xl font-bold">{data.nombre}</h2>
				</div>
				<div class="flex justify-center gap-x-4 gap-y-2 items-center mb-4 flex-wrap">
					{#each data.tipos as tipo}
						<p
							class="text-lg font-semibold px-2 py-1 text-pretty rounded-2xl"
							style="background-color: {typeColors[tipo]};"
						>
							{tipo}
						</p>
					{/each}
				</div>
				<div class="flex gap-4 justify-center items-center mb-4">
					<p class="text-sm font-semibold bg-gray-400 rounded-2xl px-2 py-1">{data.peso / 10}kg</p>
					<p class="text-sm font-semibold bg-gray-400 rounded-2xl px-2 py-1">{data.altura / 10}m</p>
				</div>
				<button
					on:click={mostrarStats}
					class="bg-blue-600 text-white font-bold text-lg py-2 px-4 rounded-2xl hover:bg-blue-500 duration-200 mb-2 sm:hidden"
					>Mostrar Stats</button
				>
				{#if isShowStats}
					<div class=" sm:hidden grid grid-cols-1 gap-y-2 w-[80%] mx-auto">
						<div class="flex flex-col items-start gap-y-2">
							<p class="bg-[#69dc12] rounded-lg px-4 py-2 text-[12px] font-bold w-full">
								Vida: {data.estadisticas[0]}
							</p>
							<p class="bg-[#efcc18] rounded-lg px-4 py-2 text-[12px] font-bold w-full">
								Ataque: {data.estadisticas[1]}
							</p>
							<p class="bg-[#e86412] rounded-lg px-4 py-2 text-[12px] font-bold w-full">
								Defensa: {data.estadisticas[2]}
							</p>
						</div>

						<div class="flex flex-col items-end gap-y-2">
							<p class="bg-[#14c3f1] rounded-lg px-4 py-2 text-[12px] font-bold w-full">
								Ataque Especial: {data.estadisticas[3]}
							</p>
							<p class="bg-[#4a6adf] rounded-lg px-4 py-2 text-[12px] font-bold w-full">
								Defensa Especial: {data.estadisticas[4]}
							</p>
							<p class="bg-[#d51dad] rounded-lg px-4 py-2 text-[12px] font-bold w-full">
								Velocidad: {data.estadisticas[5]}
							</p>
						</div>
					</div>
				{/if}
			</div>
		{:catch}
			<div class="bg-red-400 rounded h-40 w-full flex justify-center items-center">
				<p class="text-3xl font-bold text-center">Pokemon No Encontrado</p>
			</div>
		{/await}
	{:else}
		<div class="flex justify-center items-center">
			<p class="text-xl font-bold text-center">
				O prueba tambien ingresando su codigo referente a la pokédex :D
			</p>
		</div>
	{/if}
</div>
