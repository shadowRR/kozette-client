<template>

    <img id="logo" v-bind:src="logoIsMuted" alt="logo kozette">

    <!-- server -->
    <span class="oneliner">// Server -----------------------------------------------------------</span>
    Server :
    <span v-if="isServerConnected" class="green-text">connected</span>
    <span v-else class="red-text">offline</span>

    <!-- account -->
    <span class="oneliner">// Account -----------------------------------------------------------</span>
    <a class="red-link" v-on:click="logout">[ Logout ]</a>

</template>

<script type="text/babel">

    // lib
    import _ from 'lodash';
    // feathers
    import { feathers_socket, userService } from '../../services';
    // vuex
    import { loginCurrentUser, logoutCurrentUser } from '../../vuex/actions/users';
    import { currentUser, getUsersList } from '../../vuex/getters/users';
    import { isServerConnected } from '../../vuex/getters/server';

    export default {

        data() {
            return {
                interval: false
            }
        },

        vuex: {

            getters: { currentUser, getUsersList, isServerConnected },

            actions: { logoutCurrentUser, loginCurrentUser }

        },

        ready() {
            this.keepUserOnline();
        },

        computed: {

            /**
             * @summary return proper logo based on muted value
             * @return {string}
             */
            logoIsMuted() {
                const user = _.find( this.getUsersList, user => user._id == this.currentUser );
                return user && user.status.muted ?
                        '/static/img/logo/kozette_large_nosound.png' :
                        '/static/img/logo/kozette_large_transparent.png';
            }

        },

        methods: {

            /**
             * @summary patch the user online status on regular interval
             */
            keepUserOnline() {
                const patchUser = () => {
                    if ( feathers_socket.io.connected && this.currentUser )
                        userService.patch( this.currentUser, { 'status.online': true, 'status.lastSeen': new Date() } )
                            .catch( err => console.error( err ) );
                };

                patchUser();
                this.interval = setInterval( () => patchUser(), 1000 * 10 );
            },

            /**
             * @summary logout the user
             */
            logout() {

                // logout from feathers token
                feathers_socket.logout();

                // reset state
                this.logoutCurrentUser();

                // clear the user status interval check
                clearInterval( this.interval );
                this.$router.go( { name: 'login' } );
            }
            
        }
        
    };

</script>