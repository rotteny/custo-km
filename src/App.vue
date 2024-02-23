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
                        <label class="block mr-2">Automóvel de Referência</label>
                        <Dropdown 
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
                        <label class="block mr-2">Taxa do Aplicativo</label>
                        <InputNumber 
                            class="w-full"
                            v-model="aplicativo" 
                            mode="decimal" 
                            :minFractionDigits="2"
                            suffix="%"
                        />
                    </div>
                    <small class="block text-xs text-gray-400 mt-2">Taxa cobrada pelo aplicativo. Corridas feitas no particular podem usar taxa de 0% para calculo do custo.</small>
                </div>
            </div>

            <div class="p-2 mb-220px" >
                <div class="border-round border-1 border-primary p-2 mb-2">
                    <div>
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Valor do veículo</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.valor"
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor atual do veículo para calculo de depreciação.</small>
                        </div>
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Depreciacao do veículo</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.depreciacao" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    suffix="%"
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Percentual de depreciação do veículo. Vaículos em aplicativos chegam a depreciar 20% por ano.</small>
                        </div>
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Financiamento/Consórcio</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.financiamento" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor da prestação do financiamento ou consórcio que esteja pagando.</small>
                        </div>
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Km/Mês</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.km_mes" 
                                    suffix="Km" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Média de Km percorridos durante um mês.</small>
                        </div>
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Custo do veículo por Km</label>
                                <InputNumber 
                                    v-model="automovel.custo_veiculo" 
                                    :value="custoVeiculo(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo do veículo por Km percorrido.</small>
                        </div>
                    </div>
    
                    <div class="mx-2">
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Liscenciamento</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.licenciamento" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor anual cobrado pelo com IPVA e seguro obrigatório.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Seguro Anual</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.seguro" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor total anual gasto em seguro particular.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Custo com terceiros por Km</label>
                                <InputNumber 
                                    v-model="automovel.custo_terceiro" 
                                    :value="custoTerceiro(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com terceiros por Km percorrido.</small>
                        </div>
                    </div>
    
                    <div class="mx-2">
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Custo com troca de pneus</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.pneus_custo" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Inclua o custo válvulas, alinhamento e balanceamento.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Vida útil dos pneus</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.pneus_vida" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    suffix="Km"
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Quantidade máxima de Km percorridos para a troca dos pneus.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Custo com pneus por Km</label>
                                <InputNumber 
                                    v-model="automovel.custo_pneus" 
                                    :value="custoPneus(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com terceiros por Km percorrido.</small>
                        </div>
                    </div>
                    
                    <div class="mx-2">
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Valor da troca de óleo</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.oleo_custo" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Inclua o custo com óleo, filtro de óleo, filtro de ar e demais peças.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Frequência da troca de óleo</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.oleo_frequencia" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    suffix="Km"
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Em quantos Km é feita a troca do óleo.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Custo com troca de óleo por Km</label>
                                <InputNumber 
                                    v-model="automovel.custo_oleo" 
                                    :value="custoOleo(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com terceiros por Km percorrido.</small>
                        </div>
                    </div>
    
                    <div class="mx-2">
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Manutenção anual</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.outros" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Gasto anual com peças e mão de obra, oriumdos de desgastes e acidentes.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Custo com manutenção por Km</label>
                                <InputNumber 
                                    v-model="automovel.custo_outros" 
                                    :value="custoOutros(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com despesas de manutenção do veículo.</small>
                        </div>
                    </div>
    
                    <div>
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Luco/Salário</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.lucro" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Remuneração mínima esperada pelo trabalho mensal.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Custo por Km com lucro</label>
                                <InputNumber 
                                    v-model="automovel.custo_lucro" 
                                    :value="custoLucro(automovel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
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
                                <label class="block mr-2">Valor médio da gasolina</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="combustivel.gasolina.valor" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor médio da gasolina.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Consumo médio do veículo</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.consumo_gasolina" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    suffix="Km/L"
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Consumo médio do veículo com este combustível.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Custo por Km com gasolina</label>
                                <InputNumber 
                                    v-model="automovel.custo_km_gasolina"
                                    :value="custoCombustivel('consumo_gasolina', 'custo_km_gasolina', automovel, combustivel.gasolina)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com despesas de gasolina do veículo.</small>
                        </div>
                    </div>
    
                    <div class="mx-2">
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Valor médio da alcool</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="combustivel.alcool.valor" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor médio da alcool.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Consumo médio do veículo</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.consumo_alcool" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    suffix="Km/L"
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Consumo médio do veículo com este combustível.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Custo por Km com alcool</label>
                                <InputNumber 
                                    v-model="automovel.custo_km_alcool"
                                    :value="custoCombustivel('consumo_alcool', 'custo_km_alcool', automovel, combustivel.alcool)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com despesas de alcool do veículo.</small>
                        </div>
                    </div>
    
                    <div class="mx-2">
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Valor médio da gas</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="combustivel.gas.valor" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor médio da gas.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Consumo médio do veículo</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.consumo_gas" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    suffix="Km/L"
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Consumo médio do veículo com este combustível.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Custo por Km com gas</label>
                                <InputNumber 
                                    v-model="automovel.custo_km_gas"
                                    :value="custoCombustivel('consumo_gas', 'custo_km_gas', automovel, combustivel.gas)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                    :readonly="true" />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Custo com despesas de gas do veículo.</small>
                        </div>
                    </div>
    
                    <div>
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Valor médio da disel</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="combustivel.disel.valor" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Valor médio da disel.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Consumo médio do veículo</label>
                                <InputNumber 
                                    class="w-full"
                                    v-model="automovel.consumo_disel" 
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    suffix="Km/L"
                                />
                            </div>
                            <small class="block text-xs text-gray-400 mt-2">Consumo médio do veículo com este combustível.</small>
                        </div>
    
                        <div class="my-2">
                            <div>
                                <label class="block mr-2">Custo por Km com disel</label>
                                <InputNumber 
                                v-model="automovel.custo_km_disel"
                                    :value="custoCombustivel('consumo_disel', 'custo_km_disel', automovel, combustivel.disel)"
                                    class="disabled w-full"
                                    mode="decimal" 
                                    :minFractionDigits="2"
                                    prefix="R$ "
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
                            <label class="w-3 p-2">Gasolina</label>
                            <InputNumber 
                                v-model="automovel.custo_gasolina"
                                :value="custoKmCombustiel('custo_km_gasolina', 'custo_gasolina', automovel, combustivel.gasolina, aplicativo)"
                                class="w-full disabled"
                                mode="decimal" 
                                :minFractionDigits="2"
                                prefix="R$ "
                                :readonly="true" />
                        </div>
                    </div>
    
                    <div class="mb-1">
                        <div class="flex">
                            <label class="w-3 p-2">Alcool</label>
                            <InputNumber 
                                v-model="automovel.custo_alcool"
                                :value="custoKmCombustiel('custo_km_alcool', 'custo_alcool', automovel, combustivel.alcool, aplicativo)"
                                class="w-full disabled"
                                mode="decimal" 
                                :minFractionDigits="2"
                                prefix="R$ "
                                :readonly="true" />
                        </div>
                    </div>
    
                    <div class="mb-1">
                        <div class="flex">
                            <label class="w-3 p-2">Gás</label>
                            <InputNumber 
                                v-model="automovel.custo_gas"
                                :value="custoKmCombustiel('custo_km_gas', 'custo_gas', automovel, combustivel.gas, aplicativo)"
                                class="w-full disabled"
                                mode="decimal" 
                                :minFractionDigits="2"
                                prefix="R$ "
                                :readonly="true" />
                        </div>
                    </div>
    
                    <div class="mb-1">
                        <div class="flex">
                            <label class="w-3 p-2">Disel</label>
    
                            <InputNumber 
                                v-model="automovel.custo_disel"
                                :value="custoKmCombustiel('custo_km_disel', 'custo_disel', automovel, combustivel.disel, aplicativo)"
                                class="w-full disabled"
                                mode="decimal" 
                                :minFractionDigits="2"
                                prefix="R$ "
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
                aplicativo: 10,
                combustivel: {
                    gasolina: {
                        valor: 5.3,
                    },
                    alcool: {
                        valor: 3.8,
                    },
                    gas: {
                        valor: 10,
                    },
                    disel: {
                        valor: 5,
                    },
                },
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

                        outros: 0,
                        custo_outros: 0,

                        consumo_gasolina: 0,
                        custo_km_gasolina: 0,
                        consumo_alcool: 0,
                        custo_km_alcool: 0,
                        consumo_gas: 0,
                        custo_km_gas: 0,
                        consumo_disel: 0,
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

                        outros: 5000,
                        custo_outros: 0,

                        consumo_gasolina: 11.5,
                        custo_km_gasolina: 0,
                        consumo_alcool: 8.05,
                        custo_km_alcool: 0,
                        consumo_gas: 0,
                        custo_km_gas: 0,
                        consumo_disel: 0,
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

                        outros: 1500,
                        custo_outros: 0,

                        consumo_gasolina: 22,
                        custo_km_gasolina: 0,
                        consumo_alcool: 0,
                        custo_km_alcool: 0,
                        consumo_gas: 0,
                        custo_km_gas: 0,
                        consumo_disel: 0,
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
            custoCombustivel(km_field, custo_fild, veiculo, combustivel) {
                veiculo[custo_fild] = veiculo[km_field]?combustivel.valor/veiculo[km_field]:0;
            },
            custoKmCombustiel(combustivel_km, custo_field,veiculo, combustivel, aplicativo) {
                veiculo[custo_field] = (veiculo.custo_veiculo + 
                                             veiculo.custo_terceiro + 
                                             veiculo.custo_pneus + 
                                             veiculo.custo_oleo + 
                                             veiculo.custo_outros + 
                                             veiculo.custo_lucro + 
                                             veiculo[combustivel_km] ) * (1 + (aplicativo/100));
            },
        }
    }
</script>

<style>
    .disabled.p-inputnumber .p-inputtext {
        background-color: var(--yellow-300);
    }
</style>
