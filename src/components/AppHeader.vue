<template>
    <div>
        <b-navbar>
            <template slot="brand">
                <b-navbar-item :to="{ path: '/' }" tag="router-link">
                    <img alt="iPass Logo" src="../assets/logo.png" />
                </b-navbar-item>
            </template>
            <template slot="start">
                <div class="is-flex ai-center">
                    <div class="seperator"></div>
                    <div class="title is-4">Approver Dashboard</div>
                </div>
            </template>

            <template slot="end">
                <b-navbar-item tag="div">
                    <b-dropdown aria-role="list" position="is-bottom-left">
                        <button class="button is-outlined" slot="trigger">
                            <span>{{ selectedLang }}</span>
                            <b-icon icon="menu-down"></b-icon>
                        </button>

                        <b-dropdown-item
                            :key="i"
                            aria-role="listitem"
                            v-for="(item, i) in langs"
                            >{{ item }}</b-dropdown-item
                        >
                    </b-dropdown>

                    <b-dropdown aria-role="list" position="is-bottom-left">
                        <button class="button is-outlined" slot="trigger">
                            <span>Account</span>
                            <b-icon icon="menu-down"></b-icon>
                        </button>

                        <b-dropdown-item disabled>
                            <b>Logged in as {{ user.name }}</b>
                            <p>{{ user.email }}</p>
                        </b-dropdown-item>
                        <hr class="dropdown-divider" />

                        <b-dropdown-item
                            @click="logout"
                            aria-role="menuitem"
                            value="logout"
                        >
                            <div class="is-flex dropdown-menu-item">
                                <b-icon icon="logout"></b-icon>
                                <span>Sign out</span>
                            </div>
                        </b-dropdown-item>
                    </b-dropdown>
                </b-navbar-item>
            </template>
        </b-navbar>
    </div>
</template>

<script>
import { clearSession } from '../utils/session';
import EPassService from '../service/EPassService';
export default {
    name: 'AppHeader',
    data() {
        return {
            selectedLang: 'En',
            langs: ['Hi'],
            user: {
                name: '',
                email: ''
            }
        };
    },
    methods: {
        logout() {
            clearSession();
            localStorage.clear();
            window.location.reload();
        },
        async fetchProfile() {
            const { data } = await EPassService.getApproverUserProfile();
            this.user = data;
        }
    },

    created() {
        this.fetchProfile();
    }
};
</script>

<style lang="scss">
.seperator {
    width: 2px;
    height: 34px;
    background-color: #ededed;
    margin: 0 16px;
}

.navbar {
    padding: 8px 0;
}

.navbar-brand {
    padding-left: 30px;
}

.navbar-end {
    padding-right: 30px;
}

.dropdown-menu-item {
    .icon {
        margin-right: 8px;
    }
}

.dropdown-item.is-disabled {
    pointer-events: none;
}
</style>
