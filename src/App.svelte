<script>
	import {v4} from 'uuid'

	let products = [
		{
			id: 1,
			name: "Teste",
			category: "teste 2",
			description: "Teste 3",
		},
		{
			id: 2,
			name: "Test",
			category: "test 2",
			description: "Test 3",
		},
	]

	let product = {
		id: "",
		name: "",
		category: "",
		description: "",
		imageURL: "",
	}	

	let editStatus = false;

	const cleanForm = () => {
		product = {
			id: "",
			name: "",
			category: "",
			description: "",
			imageURL: "",
		}	
	}	

	const addProduct = () => {
		const newProduct = {
			id: v4(),
			name: product.name,
			category: product.category,
			description: product.description,
			imageURL: product.imageURL
		}

		products = products.concat(newProduct)
		cleanForm();
	}

	const updateProduct = () =>{
		let updatedProduct ={
			id: product.id,
			name: product.name,
			category: product.category,
			description: product.description,
			imageURL: product.imageURL
		}

		const productIndex = products.findIndex(p => p.id === product.id)
		products[productIndex] = updatedProduct;
		editStatus = false;
		cleanForm();
	}

	const onSubmitHandler = getData =>{
		if(!editStatus){
			addProduct();
		}else{
			updateProduct();
		}
	}

	const deleteProduct = (id) =>{
		products = products.filter(product => product.id !== id);
	}

	const editProduct = productEdited => {
		product = productEdited;
		editStatus = true;
	}
</script>

<main>
	<div class="container p-4">
		<div class="row">
			<div class="col-md-6">
				{#each products as product}
					<div class="card mt-2">
						<div class="">
							<div class="col-md-4">
								{#if !product.imageURL}
									<img src="images/no-product.png" alt="" class="img-fluid p-2"/>
								{:else}
									<img src="{product.imageURL}" alt="" class="img-fluid p-2"/>
								{/if}
							</div>
							<div class="col-md-8">
							<div class="card-body">
								<h5>
									<strong>{product.name}</strong>
									<span>
										<small>{product.category}</small>
									</span>
								</h5>
								<p class="card-text">{product.description}</p>
								<button class="btn btn-secondary" on:click={editProduct(product)}>Edit</button>
								<button class="btn btn-danger" on:click={deleteProduct(product.id)}>Delete</button>
							</div>
							</div>
						</div>
					</div>
					{/each}
			</div>
			<div class="col-md-6">
				<div class="card">
					<div class="card-body">
						<form on:submit|preventDefault={onSubmitHandler}>
							<input bind:value={product.name} type="text" placeholder="Product Name" id="product-name" class="form-control">
							<br/>
							<textarea bind:value={product.description} id="product-description" rows="3" placeholder="Product Description" class="form-control"></textarea>
							<br/>
							<input bind:value={product.imageURL} type="url" id="product-image-url" placeholder="Image Link" class="form-control">
							<br/>
							<select bind:value={product.category} id="category" class="form-control">
								<option value="computers">Computers</option>
								<option value="laptops">Laptops</option>
								<option value="peripherials">Peripherials</option>
							</select>
							<br/>

							<button class="btn btn-secondary">
								{#if !editStatus}Save Product {:else} Update {/if}
							</button>
						</form>
					</div>
				</div>
			</div>
		</div>
	</div>
</main>

<style>
</style>