<template>
    <section>

        <section class="panel sub-menu">

            <section class="head">

            </section>

            <section class="items-list scrollable">
                <menu-search></menu-search>
                <section class="item" :class="{'active':selectedAccount && selectedAccount.unique() === account.unique()}" v-for="account in eosAccounts" @click="selectedAccount = account">
                    <figure class="title">{{account.name}}</figure>
                    <figure class="description">Account: <b>{{account.keypair().name}}</b></figure>
                    <figure class="description">{{account.formattedWithNetwork()}}</figure>
                </section>
            </section>

        </section>

        <section class="panel display">
            <transition name="slide-right">
                <transfer :key="selectedAccount.unique()" :account="selectedAccount" v-if="selectedAccount"></transfer>
            </transition>
        </section>

    </section>
</template>

<script>
    import { mapActions, mapGetters, mapState } from 'vuex'
    import * as Actions from '../store/constants';
    import {Countries} from '../data/Countries'

    import Identity from '../models/Identity'

    export default {
        name: 'Identities',
        data () {return {
            selectedAccount:null,
        }},
        computed: {
            ...mapState([
                'scatter',
                'searchTerms',
            ]),
            ...mapGetters([
                'accounts',
                'networks',
            ]),
            eosAccounts(){
                return this.accounts.filter(x => x.blockchain() === 'eos')
                    .filter(x => x.name.toLowerCase().indexOf(this.searchTerms.toLowerCase()) > -1 ||
                            x.publicKey.toLowerCase().indexOf(this.searchTerms.toLowerCase()) > -1);
            },
        },
        mounted(){
            if(this.accounts.length) this.selectedAccount = this.accounts[0];
        },
        methods: {

        }
    }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
    @import "../_variables.scss";


</style>