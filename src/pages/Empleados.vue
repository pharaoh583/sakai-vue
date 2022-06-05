<template>
	<div class="grid">
		<div class="col-12">
			<div class="card">
				<Toast/>

				<DataTable ref="dt" :value="empleados" dataKey="id" :paginator="true" :rows="10" :filters="filters"
							paginatorTemplate="FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink CurrentPageReport RowsPerPageDropdown" :rowsPerPageOptions="[5,10,25]"
							currentPageReportTemplate="Showing {first} to {last} of {totalRecords} products" responsiveLayout="scroll">
					<template #header>
						<div class="flex flex-column md:flex-row md:justify-content-between md:align-items-center">
							<h5 class="m-0">Lista Empleados</h5>
						</div>
					</template>

					<Column field="id" header="ID" :sortable="true" headerStyle="width:14%; min-width:10rem;">
						<template #body="slotProps">
							<span class="p-column-title">ID</span>
							{{slotProps.data.id}}
						</template>
					</Column>
					<Column field="name" header="Nombre" :sortable="true" headerStyle="width:14%; min-width:10rem;">
						<template #body="slotProps">
							<span class="p-column-title">Name</span>
							{{slotProps.data.name}}
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
			empleados: null,
		}
	},
	created() {
		axios
        .get('http://localhost:8001/api/empleado')
        .then(response => {
			console.log(response);
			this.empleados = response.data.content;
		});
		console.log(this.empleados);
		// this.productService = new ProductService();
		// this.initFilters();
	},
	methods: {
	}
}
</script>

<style scoped lang="scss">
@import '../assets/demo/badges.scss';
</style>
