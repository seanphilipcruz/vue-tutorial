<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username">@{{ user.username }}</h1>
            <div class="user-profile__admin-badge" v-if="user.isAdmin">
                Admin
            </div>
            <div class="user-profile__follower-count">
                <strong>Followers: </strong> {{ followers }}
            </div>
            <form class="user-profile__create-twoot" action="" @submit.prevent="createNewTwoot">
                <label for="newTwoot"><strong>New Twoot</strong></label>
                <textarea name="newTwoot" id="newTwoot" rows="4" v-model="newTwootContent"></textarea>

                <div class="user-profile__create-twoot-type">
                    <label for="newTwootType"><strong>Type:</strong></label>
                    <select name="newTwootType" id="newTwootType" v-model="selectedTwootType">
                        <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
                            {{ option.name }}
                        </option>
                    </select>
                </div>

                <button>Twoot!</button>
            </form>
        </div>

        <div class="user-profile__twoots-wrapper">
            <Twoot
                v-for="twoot in user.twoots"
                :key="twoot.id"
                :username="user.username"
                :twoot="twoot"
                @favourite="toggleFavourite"
            />
        </div>
    </div>
</template>

<script>
import Twoot from "./Twoot";

export default {
    name: "UserProfile",
    components: {
        Twoot
    },
    data() {
        return {
            newTwootContent: '',
            selectedTwootType: 'instant',
            twootTypes: [
                { value: 'draft', name: 'Draft' },
                { value: 'instant', name: 'Instant Twoot' }
            ],
            followers: 0,
            user: {
                id: 1,
                username: '_snplpcrz',
                firstName: 'Sean',
                lastName: 'Cruz',
                email: 'seanphilipcruz@gmail.com',
                isAdmin: true,
                twoots: [
                    { id: 1, content: 'Twotter is amazing!' },
                    { id: 2, content: "Don't forget to subscribe to The Earth is Square" },
                ],
            }
        }
    },
    watch: {
        followers(newFollowerCount, oldFollowerCount) {
            if(oldFollowerCount < newFollowerCount) {
                console.log(`${this.user.username} has gained a follower!`);
            }
        }
    },
    computed: {
        fullName() {
            return `${this.user.firstName} ${this.user.lastName}`;
        }
    },
    methods: {
        followUser() {
            this.followers++
        },
        // eslint-disable-next-line no-unused-vars
        toggleFavourite(id) {
            console.log(`Favourited Tweet #${id}`)
        },
        createNewTwoot(){
            if(this.newTwootContent && this.selectedTwootType !== 'draft') {
                this.user.twoots.unshift({
                    id: this.user.twoots.length + 1,
                    content: this.newTwootContent,
                });
                this.newTwootContent = '';
            }
        }
    },
    mounted() {
        this.followUser();
    }
}
</script>

<style scoped>
.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    padding: 50px 5%;
}

.user-profile__user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #DFE3E8;
}

.user-profile__admin-badge {
    background-color: rebeccapurple;
    color: white;
    border-radius: 5px;
    margin-right: auto;
    padding: 0 10px;
    font-weight: bold;
}

.user-profile__create-twoot {
    padding-top: 20px;
    display: flex;
    flex-direction: column;
}

h1 {
    margin: 0;
}
</style>