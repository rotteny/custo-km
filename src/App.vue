<template>
    <div>
        <div class="header w-full text-center p-2">
            <div class="title text-4xl font-bold text-blue-500 w-full">
                <i class="pi pi-car text-4xl"></i> Quanto Custa o Seu Km?
            </div>
            <small class="text-xs">
                Produzido por George Rotteny <br>
                grotteny@hotmail.com
            </small>
        </div>
    
        <div class="content text-sm">
            <div class="p-2">
                <div class="mt-2">
                    <div>
                        <label for="automovel" class="block mr-2">Automóvel de Referência</label>
                        <Dropdown 
                            inputId="automovel"
                            class="w-full"
                            v-model="selected" 
                            :options="list" 
                            optionLabel="label" 
                            placeholder="Selecione o Automóvel" 
                            @change="onChangeAutomovel(selected)" />
                    </div>
                    <small class="block text-xs text-gray-400 mt-2">Veículo de referência para cálculo de custo por Km</small>
                </div>
            </div>

            <div class="p-2">
                <div class="mb-2">
                    <div>
                        <label for="aplicativo" class="block mr-2">Taxa do Aplicativo</label>
                        <InputNumber 
                            inputId="aplicativo"
                            class="w-full"
                            v-model="automovel.aplicativo" 
                            mode="decimal" 
                            suffix="%"
                            :minFractionDigits="2" />
                    </div>
                    <small class="block text-xs text-gray-400 mt-2">Taxa cobrada pelo aplicativo.</small>
                </div>
            </div>

            <div class="p-2 mb-220px" >
                <div class="border-round border-1 border-primary p-2 mb-2">
                    <div>
                        <div class="my-2">
                            <div>
                                <label for="valor" class="block mr-2">Valor do veículo</label>
                                <InputNumber 
                                    inputId="valor"
                                    class="w-full"
                                    v-model="automovel.valor"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor atual do veículo para calculo de depreciação.</small>
                        </div>
                        <div class="my-2">
                            <div>
                                <label for="depreciacao" class="block mr-2">Depreciacao do veículo</label>
                                <InputNumber 
                                    inputId="depreciacao"
                                    class="w-full"
                                    v-model="automovel.depreciacao" 
                                    mode="decimal" 
                                    suffix="%"
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Percentual de depreciação do veículo. Vaículos em aplicativos chegam a depreciar 20% por ano.</small>
                        </div>
                        <div class="my-2">
                            <div>
                                <label for="financiamento" class="block mr-2">Financiamento/Consórcio</label>
                                <InputNumber 
                                    inputId="financiamento"
                                    class="w-full"
                                    v-model="automovel.financiamento" 
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor da prestação do financiamento ou consórcio que esteja pagando.</small>
                        </div>
                        <div class="my-2">
                            <div>
                                <label for="km_mes" class="block mr-2">Km/Mês</label>
                                <InputNumber 
                                    inputId="km_mes"
                                    class="w-full"
                                    v-model="automovel.km_mes" 
                                    suffix="Km" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Média de Km percorridos durante um mês.</small>
                        </div>
                        <div class="my-2">
                            <div>
                                <label for="custo_veiculo" class="block mr-2">Custo do veículo por Km</label>
                                <InputNumber 
                                    inputId="custo_veiculo"
                                    v-model="automovel.custo_veiculo" 
                                    :value="custoVeiculo(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo do veículo por Km percorrido.</small>
                        </div>
                    </div>
    
                    <div class="mx-2">
                        <div class="my-2">
                            <div>
                                <label for="licenciamento" class="block mr-2">Liscenciamento</label>
                                <InputNumber 
                                    inputId="licenciamento"
                                    class="w-full"
                                    v-model="automovel.licenciamento" 
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor anual cobrado pelo com IPVA e seguro obrigatório.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="seguro" class="block mr-2">Seguro Anual</label>
                                <InputNumber 
                                    inputId="seguro"
                                    class="w-full"
                                    v-model="automovel.seguro" 
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor total anual gasto em seguro particular.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="custo_terceiro" class="block mr-2">Custo com terceiros por Km</label>
                                <InputNumber 
                                    inputId="custo_terceiro"
                                    v-model="automovel.custo_terceiro" 
                                    :value="custoTerceiro(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com terceiros por Km percorrido.</small>
                        </div>
                    </div>
    
                    <div class="mx-2">
                        <div class="my-2">
                            <div>
                                <label for="pneus_custo" class="block mr-2">Custo com troca de pneus</label>
                                <InputNumber 
                                    inputId="pneus_custo"
                                    class="w-full"
                                    v-model="automovel.pneus_custo" 
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Inclua o custo válvulas, alinhamento e balanceamento.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="pneus_vida" class="block mr-2">Vida útil dos pneus</label>
                                <InputNumber 
                                    inputId="pneus_vida"
                                    class="w-full"
                                    v-model="automovel.pneus_vida" 
                                    mode="decimal" 
                                    suffix="Km"
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Quantidade máxima de Km percorridos para a troca dos pneus.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="custo_pneus" class="block mr-2">Custo com pneus por Km</label>
                                <InputNumber 
                                    inputId="custo_pneus"
                                    v-model="automovel.custo_pneus" 
                                    :value="custoPneus(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com terceiros por Km percorrido.</small>
                        </div>
                    </div>
                    
                    <div class="mx-2">
    
                        <div class="my-2">
                            <div>
                                <label for="oleo_custo" class="block mr-2">Valor da troca de óleo</label>
                                <InputNumber 
                                    inputId="oleo_custo"
                                    class="w-full"
                                    v-model="automovel.oleo_custo" 
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Inclua o custo com óleo, filtro de óleo, filtro de ar e demais peças.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="oleo_frequencia" class="block mr-2">Frequência da troca de óleo</label>
                                <InputNumber 
                                    inputId="oleo_frequencia"
                                    class="w-full"
                                    v-model="automovel.oleo_frequencia" 
                                    mode="decimal" 
                                    suffix="Km"
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Em quantos Km é feita a troca do óleo.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="custo_oleo" class="block mr-2">Custo com troca de óleo por Km</label>
                                <InputNumber 
                                    inputId="custo_oleo"
                                    v-model="automovel.custo_oleo" 
                                    :value="custoOleo(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com terceiros por Km percorrido.</small>
                        </div>
                    </div>
    
                    <div class="mx-2">
                        <div class="my-2">
                            <div>
                                <label for="outros" class="block mr-2">Manutenção anual</label>
                                <InputNumber 
                                    inputId="outros"
                                    class="w-full"
                                    v-model="automovel.outros" 
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Gasto anual com peças e mão de obra, oriumdos de desgastes e acidentes.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="custo_outros" class="block mr-2">Custo com manutenção por Km</label>
                                <InputNumber 
                                    inputId="custo_outros"
                                    v-model="automovel.custo_outros" 
                                    :value="custoOutros(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com despesas de manutenção do veículo.</small>
                        </div>
                    </div>
    
                    <div>
                        <div class="my-2">
                            <div>
                                <label for="lucro" class="block mr-2">Luco/Salário</label>
                                <InputNumber 
                                    inputId="lucro"
                                    class="w-full"
                                    v-model="automovel.lucro" 
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Remuneração mínima esperada pelo trabalho mensal.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="custo_lucro" class="block mr-2">Custo por Km com lucro</label>
                                <InputNumber 
                                    inputId="custo_lucro"
                                    v-model="automovel.custo_lucro" 
                                    :value="custoLucro(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com despesas de manutenção do veículo.</small>
                        </div>
                    </div>
                </div>
            
                <div class="border-round border-1 border-primary p-2">
                    <div>
                        <div class="my-2">
                            <div>
                                <label for="gasolina_custo" class="block mr-2">Valor médio da gasolina</label>
                                <InputNumber 
                                    inputId="gasolina_custo"
                                    class="w-full"
                                    v-model="automovel.gasolina_custo" 
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor médio da gasolina.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="gasolina_km" class="block mr-2">Consumo médio do veículo</label>
                                <InputNumber 
                                    inputId="gasolina_km"
                                    class="w-full"
                                    v-model="automovel.gasolina_km" 
                                    mode="decimal" 
                                    suffix="Km/l"
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Consumo médio do veículo com este combustível.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="custo_gasolina" class="block mr-2">Custo por Km com gasolina</label>
                                <InputNumber 
                                    inputId="custo_gasolina"
                                    v-model="automovel.custo_gasolina" 
                                    :value="custoGasolina(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com despesas de gasolina do veículo.</small>
                        </div>
                    </div>
    
                    <div class="mx-2">
    
                        <div class="my-2">
                            <div>
                                <label for="alcool_custo" class="block mr-2">Valor médio da alcool</label>
                                <InputNumber 
                                    inputId="alcool_custo"
                                    class="w-full"
                                    v-model="automovel.alcool_custo" 
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor médio da alcool.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="alcool_km" class="block mr-2">Consumo médio do veículo</label>
                                <InputNumber 
                                    inputId="alcool_km"
                                    class="w-full"
                                    v-model="automovel.alcool_km" 
                                    mode="decimal" 
                                    suffix="Km/l"
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Consumo médio do veículo com este combustível.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="custo_alcool" class="block mr-2">Custo por Km com alcool</label>
                                <InputNumber 
                                    inputId="custo_alcool"
                                    v-model="automovel.custo_alcool" 
                                    :value="custoAlcool(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com despesas de alcool do veículo.</small>
                        </div>
                    </div>
    
                    <div class="mx-2">
                        <div class="my-2">
                            <div>
                                <label for="gas_custo" class="block mr-2">Valor médio da gas</label>
                                <InputNumber 
                                    inputId="gas_custo"
                                    class="w-full"
                                    v-model="automovel.gas_custo" 
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor médio da gas.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="gas_km" class="block mr-2">Consumo médio do veículo</label>
                                <InputNumber 
                                    inputId="gas_km"
                                    class="w-full"
                                    v-model="automovel.gas_km" 
                                    mode="decimal" 
                                    suffix="Km/l"
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Consumo médio do veículo com este combustível.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="custo_gas" class="block mr-2">Custo por Km com gas</label>
                                <InputNumber 
                                    inputId="custo_gas"
                                    v-model="automovel.custo_gas" 
                                    :value="custoGas(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com despesas de gas do veículo.</small>
                        </div>
                    </div>
    
                    <div>
                        <div class="my-2">
                            <div>
                                <label for="disel_custo" class="block mr-2">Valor médio da disel</label>
                                <InputNumber 
                                    inputId="disel_custo"
                                    class="w-full"
                                    v-model="automovel.disel_custo" 
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor médio da disel.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="disel_km" class="block mr-2">Consumo médio do veículo</label>
                                <InputNumber 
                                    inputId="disel_km"
                                    class="w-full"
                                    v-model="automovel.disel_km" 
                                    mode="decimal" 
                                    suffix="Km/l"
                                    :minFractionDigits="2" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Consumo médio do veículo com este combustível.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label for="custo_disel" class="block mr-2">Custo por Km com disel</label>
                                <InputNumber 
                                    inputId="custo_disel"
                                    v-model="automovel.custo_disel" 
                                    :value="custoDisel(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com despesas de disel do veículo.</small>
                        </div>
                    </div>
                </div>
            </div>

            <div class="surface-200 shadow-4 fixed bottom-0 w-full bot">
                <div class="p-1 overflow-auto">
                    <div class="mb-1">
                        <div class="flex">
                            <label for="custo_km_gasolina" class="w-3 p-2">Gasolina</label>
                            <InputNumber 
                                    inputId="custo_km_gasolina"
                                    v-model="automovel.custo_km_gasolina" 
                                    :value="custoKmGasolina(automovel)"
                                    class="w-full disabled"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                        </div>
                    </div>
    
                    <div class="mb-1">
                        <div class="flex">
                            <label for="custo_km_alcool" class="w-3 p-2">Alcool</label>
                            <InputNumber 
                                    inputId="custo_km_alcool"
                                    v-model="automovel.custo_km_alcool" 
                                    :value="custoKmAlcool(automovel)"
                                    class="w-full disabled"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                        </div>
                    </div>
    
                    <div class="mb-1">
                        <div class="flex">
                            <label for="custo_km_gas" class="w-3 p-2">Gás</label>
                            <InputNumber 
                                    inputId="custo_km_gas"
                                    v-model="automovel.custo_km_gas" 
                                    :value="custoKmGas(automovel)"
                                    class="w-full disabled"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                        </div>
                    </div>
    
                    <div class="mb-1">
                        <div class="flex">
                            <label for="custo_km_disel" class="w-3 p-2">Disel</label>
    
                            <InputNumber 
                                    inputId="custo_km_disel"
                                    v-model="automovel.custo_km_disel" 
                                    :value="custoKmDisel(automovel)"
                                    class="w-full disabled"
                                    mode="decimal" 
                                    prefix="R$ "
                                    :minFractionDigits="2"
                                    :readonly="true" />
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Dropdown from 'primevue/dropdown';
    import InputNumber from 'primevue/inputnumber';
    import ScrollPanel from 'primevue/scrollpanel';

    export default {
        components: {
            Dropdown, InputNumber, ScrollPanel,
        },
        beforeMount() {
            this.selected = this.list[0];
            this.onChangeAutomovel(this.selected);
        },
        data() {
            return {
                automovel: {},
                selected: {},
                list: [
                {
                        id: 'novo', 
                        label: 'Novo',
                        valor: 0,
                        depreciacao: 20,
                        financiamento: 0,
                        km_mes: 5000,
                        custo_veiculo: 0,

                        licenciamento: 0,
                        seguro: 0,
                        custo_terceiro: 0,

                        pneus_custo: 0,
                        pneus_vida: 50000,
                        custo_pneus: 0,

                        oleo_custo: 0,
                        oleo_frequencia: 10000,
                        custo_oleo: 0,

                        aplicativo: 10,

                        outros: 0,
                        custo_outros: 0,

                        gasolina_custo: 0,
                        gasolina_km: 0,
                        custo_gasolina: 0,
                        custo_km_gasolina: 0,

                        alcool_custo: 0,
                        alcool_km: 0,
                        custo_alcool: 0,
                        custo_km_alcool: 0,

                        gas_custo: 0,
                        gas_km: 0,
                        custo_gas: 0,
                        custo_km_gas: 0,

                        disel_custo: 0,
                        disel_km: 0,
                        custo_disel: 0,
                        custo_km_disel: 0,

                        lucro: 4000,
                        custo_lucro: 0,
                    },
                    {
                        id: 'uno', 
                        label: 'Fiat Uno (2008)',
                        valor: 21000,
                        depreciacao: 5,
                        financiamento: 0,
                        km_mes: 2000,
                        custo_veiculo: 0,

                        licenciamento: 160,
                        seguro: 0,
                        custo_terceiro: 0,

                        pneus_custo: 1200,
                        pneus_vida: 50000,
                        custo_pneus: 0,

                        oleo_custo: 180,
                        oleo_frequencia: 10000,
                        custo_oleo: 0,

                            aplicativo: 10,

                        outros: 5000,
                        custo_outros: 0,

                        gasolina_custo: 5.2,
                        gasolina_km: 11.5,
                        custo_gasolina: 0,
                        custo_km_gasolina: 0,

                        alcool_custo: 0,
                        alcool_km: 0,
                        custo_alcool: 0,
                        custo_km_alcool: 0,

                        gas_custo: 0,
                        gas_km: 0,
                        custo_gas: 0,
                        custo_km_gas: 0,

                        disel_custo: 0,
                        disel_km: 0,
                        custo_disel: 0,
                        custo_km_disel: 0,

                        lucro: 2000,
                        custo_lucro: 0,
                    },
                    {
                        id: 'prius', 
                        label: 'Prius (2022)',
                        valor: 100000,
                        depreciacao: 20,
                        financiamento: 1302,
                        km_mes: 5000,
                        custo_veiculo: 0,

                        licenciamento: 2150,
                        seguro: 3984,
                        custo_terceiro: 0,

                        pneus_custo: 1800,
                        pneus_vida: 50000,
                        custo_pneus: 0,

                        oleo_custo: 500,
                        oleo_frequencia: 10000,
                        custo_oleo: 0,

                            aplicativo: 10,

                        outros: 1500,
                        custo_outros: 0,

                        gasolina_custo: 4.89,
                        gasolina_km: 22,
                        custo_gasolina: 0,
                        custo_km_gasolina: 0,

                        alcool_custo: 0,
                        alcool_km: 0,
                        custo_alcool: 0,
                        custo_km_alcool: 0,

                        gas_custo: 0,
                        gas_km: 0,
                        custo_gas: 0,
                        custo_km_gas: 0,

                        disel_custo: 0,
                        disel_km: 0,
                        custo_disel: 0,
                        custo_km_disel: 0,

                        lucro: 4500,
                        custo_lucro: 0,
                    },
                ]
            }
        },
        methods: {
            onChangeAutomovel(veiculo) {
                this.automovel = {...veiculo};
            },
            custoVeiculo(veiculo) {
                const depreciacao = (veiculo.valor * veiculo.depreciacao)/100;
                const depreciacao_fim = veiculo.km_mes?depreciacao/(veiculo.km_mes*12):0;
                const parcela     = veiculo.km_mes?veiculo.financiamento/veiculo.km_mes:0;

                veiculo.custo_veiculo = depreciacao_fim+parcela;
            },
            custoTerceiro(veiculo) {
                const total     = veiculo.licenciamento + veiculo.seguro;
                const km_ano    = veiculo.km_mes*12;

                veiculo.custo_terceiro = km_ano?total/km_ano:0;
            },
            custoPneus(veiculo) {
                veiculo.custo_pneus = veiculo.pneus_vida?veiculo.pneus_custo/veiculo.pneus_vida:0;
            },
            custoOleo(veiculo) {
                veiculo.custo_oleo = veiculo.oleo_frequencia?veiculo.oleo_custo/veiculo.oleo_frequencia:0;
            },
            custoOutros(veiculo) {
                const km_ano    = veiculo.km_mes*12;
                veiculo.custo_outros = veiculo.outros/km_ano;
            },
            custoLucro(veiculo) {
                veiculo.custo_lucro = veiculo.km_mes?veiculo.lucro/veiculo.km_mes:0;
            },
            custoGasolina(veiculo) {
                veiculo.custo_gasolina = veiculo.gasolina_km?veiculo.gasolina_custo/veiculo.gasolina_km:0;
            },
            custoAlcool(veiculo) {
                veiculo.custo_alcool = veiculo.alcool_km?veiculo.alcool_custo/veiculo.alcool_km:0;
            },
            custoGas(veiculo) {
                veiculo.custo_gas = veiculo.gas_km?veiculo.gas_custo/veiculo.gas_km:0;
            },
            custoDisel(veiculo) {
                veiculo.custo_disel = veiculo.disel_km?veiculo.disel_custo/veiculo.disel_km:0;
            },
            custoKmGasolina(veiculo) {
                veiculo.custo_km_gasolina = (veiculo.custo_veiculo + 
                                             veiculo.custo_terceiro + 
                                             veiculo.custo_pneus + 
                                             veiculo.custo_oleo + 
                                             veiculo.custo_outros + 
                                             veiculo.custo_lucro + 
                                             veiculo.custo_gasolina ) * (1 + (veiculo.aplicativo/100));
            },
            custoKmAlcool(veiculo) {
                veiculo.custo_km_alcool = (veiculo.custo_veiculo + 
                                             veiculo.custo_terceiro + 
                                             veiculo.custo_pneus + 
                                             veiculo.custo_oleo + 
                                             veiculo.custo_outros + 
                                             veiculo.custo_lucro + 
                                             veiculo.custo_alcool ) * (1 + (veiculo.aplicativo/100));
            },
            custoKmGas(veiculo) {
                veiculo.custo_km_gas = (veiculo.custo_veiculo + 
                                             veiculo.custo_terceiro + 
                                             veiculo.custo_pneus + 
                                             veiculo.custo_oleo + 
                                             veiculo.custo_outros + 
                                             veiculo.custo_lucro + 
                                             veiculo.custo_gas ) * (1 + (veiculo.aplicativo/100));
            },
            custoKmDisel(veiculo) {
                veiculo.custo_km_disel = (veiculo.custo_veiculo + 
                                             veiculo.custo_terceiro + 
                                             veiculo.custo_pneus + 
                                             veiculo.custo_oleo + 
                                             veiculo.custo_outros + 
                                             veiculo.custo_lucro + 
                                             veiculo.custo_disel ) * (1 + (veiculo.aplicativo/100));
            },
        }
    }
</script>

<style>
    .disabled.p-inputnumber .p-inputtext {
        background-color: var(--yellow-300);
    }
</style>
