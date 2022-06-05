<template>
	<div class="grid">
		<div class="col-12 md:col-6">
			<div class="card p-fluid">
				<h5>Crear Cuenta</h5>
				<div class="field grid">
					<label for="name3" class="col-12 mb-2 md:col-2 md:mb-0">Cuenta origen: </label>
					<div class="col-12 md:col-10">
						<InputText  v-model="coid" id="name3" type="text" />
					</div>
				</div>
                <div class="field grid">
					<label for="name3" class="col-12 mb-2 md:col-2 md:mb-0">Cuenta Destino: </label>
					<div class="col-12 md:col-10">
						<InputText  v-model="cdid" id="name3" type="text" />
					</div>
				</div>
				<div class="field grid">
					<label for="email3" class="col-12 mb-2 md:col-2 md:mb-0">Monto</label>
					<div class="col-12 md:col-10">
						<InputText v-model="amount" id="email3" type="text" />
					</div>
				</div>
			</div>
            <Button label="Submit" @click="registrarTransaccion()"></Button>
        </div>
	</div>
	<div class="grid">
		<div class="col-12">
			<div class="card">
				<Toast/>

				<DataTable ref="dt" :value="transacciones" dataKey="id" :paginator="true" :rows="10" :filters="filters"
							paginatorTemplate="FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink CurrentPageReport RowsPerPageDropdown" :rowsPerPageOptions="[5,10,25]"
							currentPageReportTemplate="Showing {first} to {last} of {totalRecords} products" responsiveLayout="scroll">
					<template #header>
						<div class="flex flex-column md:flex-row md:justify-content-between md:align-items-center">
							<h5 class="m-0">Lista Transacciones</h5>
						</div>
					</template>

					<Column field="id" header="ID" :sortable="true" headerStyle="width:14%; min-width:10rem;">
						<template #body="slotProps">
							<span class="p-column-title">ID</span>
							{{slotProps.data.id}}
						</template>
					</Column>
					<Column field="coid" header="Cuenta Origen" :sortable="true" headerStyle="width:14%; min-width:10rem;">
						<template #body="slotProps">
							<span class="p-column-title">Name</span>
							{{slotProps.data.coid}}
						</template>
					</Column>
                    <Column field="cdid" header="Cuenta Destino" :sortable="true" headerStyle="width:14%; min-width:10rem;">
						<template #body="slotProps">
							<span class="p-column-title">Name</span>
							{{slotProps.data.cdid}}
						</template>
					</Column>
					<Column field="amount" header="Monto" :sortable="true" headerStyle="width:14%; min-width:10rem;">
						<template #body="slotProps">
							<span class="p-column-title">monto</span>
							{{slotProps.data.amount}}
						</template>
					</Column>
				</DataTable>
			</div>
		</div>
	</div>

</template>

<script>
import axios from 'axios';
export default {
	data() {
		return {
			filters: {},
			transacciones: null,
			coid: null,
            cdid: null,
            amount: null,
            empleado: 1,
		}
	},
	created(){
		axios
		.get('http://localhost:8002/api/transaccion')
		.then(response => {
			this.transacciones = response.data.content;
		})
		.catch(error => {
			console.log(error);
		});
	},
	methods: {
		async registrarTransaccion(){
			if(this.amount <= 0){
				alert("El monto no puede ser 0");
				return;
			}
			await fetch(
				`http://localhost:8003/api/cuenta`,{
				method: "POST",
				body: JSON.stringify({
                    eid: this.empleado,
					coid: this.coid,
					cdid: this.cdid,
                    amount: this.amount
				}),
				headers: { "Content-Type": "application/json" },
			})
			.then(response => {
				if(response.status === 200){
					alert("Registrado Correctamente");
					this.coid = null;
					this.cdid = null;
                    this.amount = null;
				}
				else alert("Hubo un error en el registro");
			})
			.catch(error => {
				console.log(error);
			});
		},
	}
}
</script>
