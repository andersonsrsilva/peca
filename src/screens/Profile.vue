<template>
  <div class="bg-orange h-screen grid p-8">
    <Header :title="$t('PROFILE')" />
    <div class="w-full mx-auto" uppercase>
      <form
        class="overflow-y-auto"
        uppercase
        action="#"
        method="POST"
        @submit="submit"
      >
        <div class="mx-auto">
          <file-avatar
            v-model="user.profile_img"
            :value="user?.profile_img"
            is-editable
          />
          <h3 class="text-red text-md w-full text-center font-bold my-8">
            {{ user?.nome_fornecedor }}
          </h3>

          <div
            class="text-center text-white grid justify-center grid grid-flow-row-dense grid-cols-4"
          >
            <div
              class="appearance-none col-span-4 uppercase bg-orange py-2 border-red placeholder-red text-red text-left"
            >
              {{ $t('ADDRESS') }}
            </div>

            <input
              v-model="user.dados_fornecedor.endereco.end_cep"
              v-mask="'#####-###'"
              name="cep"
              type="text"
              required
              class="appearance-none col-span-2 uppercase border bg-orange px-3 py-2 border-red placeholder-red text-red focus:outline-none focus:border-white text-sm"
              :placeholder="$t('CEP')"
              @change="getAddress"
            />

            <input
              v-model="user.dados_fornecedor.endereco.end_logradouro"
              name="logradouro"
              type="text"
              required
              class="appearance-none uppercase col-span-2 border bg-orange px-3 py-2 border-red placeholder-red text-red focus:outline-none focus:border-white text-sm"
              :placeholder="$t('PLACE')"
            />

            <!-- <select
              v-model="user.cidade"
              :placeholder="$t('CITY')"
              class="form-select form-select-sm appearance-none col-span-4 block w-full appearance-none uppercase border bg-orange px-3 py-2 border-red placeholder-red text-red focus:outline-none focus:border-white text-sm"
              aria-label=".form-select-sm example"
            >
              <option
                v-for="op in dropdownCidade"
                :key="op.cod"
                :value="op.nome"
              >
                {{ op.nome }}
              </option>
            </select> -->

            <input
              v-model="user.dados_fornecedor.endereco.end_uf_cidade_id"
              name="city"
              type="text"
              required
              class="appearance-none uppercase col-span-3 border bg-orange px-3 py-2 border-red placeholder-red text-red focus:outline-none focus:border-white text-sm"
              :placeholder="$t('CITY')"
            />

            <!-- <select
              v-model="user.uf"
              :placeholder="$t('UF')"
              class="form-select form-select-sm appearance-none col-span-4 block w-full appearance-none uppercase border bg-orange px-3 py-2 border-red placeholder-red text-red focus:outline-none focus:border-white text-sm"
              aria-label=".form-select-sm example"
              @change="getCities"
            >
              <option v-for="op in dropdownUF" :key="op.cod" :value="op">
                {{ op.cod }}
              </option>
            </select> -->

            <input
              v-model="user.dados_fornecedor.endereco.end_uf_id"
              name="uf"
              type="text"
              maxlength="2"
              required
              class="appearance-none uppercase col-span-1 border bg-orange px-3 py-2 border-red placeholder-red text-red focus:outline-none focus:border-white text-sm"
              :placeholder="$t('UF')"
            />

            <input
              v-model="user.dados_fornecedor.endereco.end_complemento"
              name="complement"
              type="text"
              required
              class="appearance-none uppercase col-span-4 border bg-orange px-3 py-2 border-red placeholder-red text-red focus:outline-none focus:border-white text-sm"
              :placeholder="$t('COMPLEMENT')"
            />

            <input
              v-model="user.dados_fornecedor.endereco.end_nr"
              name="number"
              type="text"
              required
              class="appearance-none uppercase col-span-2 border bg-orange px-3 py-2 border-red placeholder-red text-red focus:outline-none focus:border-white text-sm"
              :placeholder="$t('NUMBER')"
            />

            <input
              v-model="user.dados_fornecedor.endereco.end_bairro"
              name="district"
              type="text"
              required
              class="appearance-none uppercase col-span-2 border bg-orange px-3 py-2 border-red placeholder-red text-red focus:outline-none focus:border-white text-sm"
              :placeholder="$t('DISTRICT')"
            />

            <div
              class="appearance-none col-span-4 uppercase bg-orange py-2 border-red placeholder-red text-red text-left"
            >
              {{ $t('BANK_DATA') }}
            </div>

            <select
              v-model="user.dados_fornecedor.dados_bancarios.banco_id"
              class="form-select form-select-sm appearance-none col-span-4 block w-full appearance-none uppercase border bg-orange px-3 py-2 border-red placeholder-red text-red focus:outline-none focus:border-white text-sm"
              aria-label=".form-select-sm example"
            >
              <option v-for="op in dropdownBanco" :key="op.id" :value="op.id">
                {{ op.nome }}
              </option>
            </select>

            <input
              v-model="user.dados_fornecedor.dados_bancarios.banco_ag"
              v-mask="'####-#'"
              name="ag"
              type="text"
              required
              class="appearance-none uppercase col-span-2 border bg-orange px-3 py-2 border-red placeholder-red text-red focus:outline-none focus:border-white text-sm"
              :placeholder="$t('AGENCY')"
            />

            <input
              v-model="user.dados_fornecedor.dados_bancarios.banco_cc"
              v-mask="'######-#'"
              name="conta"
              type="text"
              required
              class="appearance-none uppercase col-span-2 border bg-orange px-3 py-2 border-red placeholder-red text-red focus:outline-none focus:border-white text-sm"
              :placeholder="$t('ACCOUNT')"
            />
          </div>
        </div>

        <div>
          <button
            type="submit"
            class="appearance-none uppercase bg-red mx-auto group relative w-20 leading-normal flex justify-center mt-8 py-2 px-16 text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
          >
            {{ $t('UPDATE') }}
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import { mapState, useStore } from 'vuex'
import FileAvatar from '@/components/FileAvatar.vue'
import Header from '@/components/Header.vue'

import { URL } from '@/api/http.js'

export default {
  components: { FileAvatar, Header },
  setup() {
    const { dispatch } = useStore()

    // const value = ref({
    //   profile_img: 'https://mdbcdn.b-cdn.net/img/new/avatars/2.webp',
    //   dados_fornecedor: {
    //     endereco: {
    //       end_logradouro: 'RUA ANTÔNIO VIEIRA FILHO',
    //       end_nr: '0',
    //       end_complemento: 'QUADRA 06 LOTE 18',
    //       end_bairro: 'SETOR LESTE',
    //       end_uf_cidade_id: '1017',
    //       end_uf_id: '9',
    //       end_cep: '72803360',
    //     },
    //     dados_bancarios: {
    //       banco_id: '1',
    //       banco_tipo_conta_id: '1',
    //       banco_ag: '12033',
    //       banco_cc: '8502595',
    //       cod_operacao: '051',
    //       chave_pix: '85541710120',
    //     },
    //   },
    // })
    dispatch('list/getList', 'auth')
    dispatch('dropdown/getDropdown', 'bancos')
    dispatch('dropdown/getDropdown', 'ufs')

    return {
      dispatch,
    }
  },
  computed: mapState({
    dropdownBanco: (state) => {
      return state.dropdown.bancos || []
    },
    dropdownUF: (state) => {
      return state.dropdown.ufs || []
    },
    dropdownCidade: (state) => {
      return state.dropdown.cidades || []
    },
    user: (state) => {
      if (state.list.auth) {
        const fornecedor = { ...state.list.auth }
        const link = fornecedor?.profile_img.slice(0, 9)
        if (link === '/storage/') {
          fornecedor.profile_img = URL + fornecedor.profile_img
        }
        return fornecedor
      }

      return []
    },
  }),

  methods: {
    getCities() {
      this.dispatch('dropdown/getCityDropdown', this.value.uf.id)
    },
    getAddress(e) {
      this.dispatch('dropdown/getCep', e.target.value).then((response) => {
        this.value.logradouro = response.address
        this.value.uf = response.state
        this.value.cidade = response.city
        this.value.complemento = response.district
      })
    },

    submit(e) {
      e.preventDefault()

      const data = {
        profile_img: this.user.profile_img,
        dados_fornecedor: this.user.dados_fornecedor,
      }

      this.dispatch('form/update', {
        value: data,
      })
    },
  },
}
</script>
