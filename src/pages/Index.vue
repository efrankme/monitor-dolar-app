<template>
	<q-page class="flex flex-start bg-blue-grey-1">
		<q-pull-to-refresh @refresh="refresh" class="panel">
			<div class="q-pa-sm">
				<q-card>
					<q-tabs
						v-model="tab"				
						class="text-blue-grey"
						active-color="blue-grey-10"
						indicator-color="blue-grey-9"
						align="justify"
					>
						<q-tab name="tasas" label="Tasas" />
						<q-tab name="convertidor" label="Convertidor" />
					</q-tabs>

					<q-separator />

					<q-tab-panels v-model="tab" animated>
						<q-tab-panel name="tasas" class="q-pa-none">
							<q-list class="text-blue-grey-10">

								<q-item v-ripple class="q-my-md">
									<q-item-section avatar>
										<q-img src="~assets/enparalelovzla.png" />
									</q-item-section>

									<q-item-section>
										<q-item-label>Monitor Dolar</q-item-label>										
										<q-item-label caption>{{tasas.enparalelo.fecha}}</q-item-label>
									</q-item-section>
									<q-item-section side>
										<b>{{ tasas.enparalelo.valor | formatoBs }}</b>
										
									</q-item-section>
								</q-item>

								<q-separator inset />

								<q-item v-ripple class="q-my-md">
									<q-item-section avatar>
										<q-img src="~assets/dolartoday.png" />
									</q-item-section>

									<q-item-section>
										<q-item-label>DolarToday</q-item-label>
										<q-item-label caption>{{tasas.dolartoday.fecha}}</q-item-label>
									</q-item-section>
									<q-item-section side>
										<b>{{ tasas.dolartoday.valor | formatoBs}}</b>
									</q-item-section>
								</q-item>

								<q-separator inset />

								<q-item v-ripple class="q-my-md">
									<q-item-section avatar>
										<q-img src="~assets/yadio.png" />
									</q-item-section>
									<q-item-section>
										<q-item-label>Yadio</q-item-label>
										<q-item-label caption>{{tasas.yadio.fecha}}</q-item-label>
									</q-item-section>
									<q-item-section side>
										<b>{{ tasas.yadio.valor | formatoBs}}</b>
									</q-item-section>
								</q-item>

								<q-separator inset/>

								<q-item v-ripple class="q-my-md">
									<q-item-section avatar>
										<q-img src="~assets/bcv.png" />
									</q-item-section>

									<q-item-section>
										<q-item-label>BCV</q-item-label>
										<q-item-label caption>{{tasas.bcv.fecha}}</q-item-label>
									</q-item-section>
									<q-item-section side>
										<b>{{ tasas.bcv.valor | formatoBs}}</b>
									</q-item-section>
								</q-item>

								<q-separator inset />

								<q-item v-ripple class="q-my-md">
									<q-item-section avatar>
										<q-img src="~assets/airtm-logo.png" />
									</q-item-section>

									<q-item-section>
										<q-item-label>AirTM</q-item-label>
										<q-item-label caption>{{tasas.airtm.fecha}}</q-item-label>
									</q-item-section>
									<q-item-section side>
										<b>{{ tasas.airtm.valor | formatoBs }}</b>
									</q-item-section>
								</q-item>
							</q-list>
						</q-tab-panel>

						<q-tab-panel name="convertidor">
						<Convertidor :tasas="tasas"/>
						</q-tab-panel>
					</q-tab-panels>
				</q-card>
			</div>
		</q-pull-to-refresh>
	</q-page>
</template>

<script>
	import Convertidor from "components/Convertidor.vue";
	import PouchDB from "pouchdb";

	const formatter = new Intl.NumberFormat("es-VE", {
		style: "decimal",
		minimumFractionDigits: 0,
	});

	let db = new PouchDB("tasas")
	var remoteDB = new PouchDB("https://43a0f91c-05b3-4963-8d34-c59ca2b48399-bluemix.cloudantnosqldb.appdomain.cloud/tasas");

	export default {
		name: "PageIndex",
		created() {
			this.sync();

			this.cargaTasas();
			
			db.changes({
				since: "now",
				live: true,
			}).on("change", this.cargaTasas);
		},
		components:{
			Convertidor
		},
		data() {
			return {
				tasas: {
					dolartoday: {},
					airtm: {},
					yadio: {},
					bcv: {},
					enparalelo: {}
				},
				tab: "tasas"
			};
		},

		methods: {

			sync(done) {
				var opts = { live: true };
				db.replicate.from(remoteDB, opts, this.syncError);
				if(done) done()
			},

			syncError() {
				// syncDom.setAttribute('data-sync-state', 'error');
			},

			async cargaTasas(done) {
				const tasas = await db.allDocs({ include_docs: true, })
					.then(docs => docs.rows)
					.catch(error => console.log("Error al consultar " + error));
				console.log(tasas);
				this.tasas.dolartoday = tasas[2].doc;
				this.tasas.airtm = tasas[0].doc;
				this.tasas.yadio = tasas[4].doc;
				this.tasas.bcv = tasas[1].doc;
				this.tasas.enparalelo = tasas[3].doc;					
			},

			refresh(done) {
				this.sync(done);
			}
		},
		filters: {
			formatoBs: (valor) =>{
				return "Bs. " + formatter.format(valor)
			}
		}
	};
</script>

<style scoped>
	.panel {
		width: 100%;
		max-width: 600px;
	}

	b {
		font-size: 1.2em;
	}

	.text-caption {
		font-size: .70rem
	}

	.q-item__section--side {
    color: inherit;
}
</style>
