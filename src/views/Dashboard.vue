<template>
  <div  class="page-bg" :key="0">
    <header class="flex">
        <nav class="border-r-4 border-blue-100" style="min-width: 125px; min-height: 100vh;">
            <!-- <div class="w-full py-10">
                <a href="#" class="ml-auto block text-center">
                    <img src="@/assets/img/communite-logo.png" alt="" class="mx-auto">
                </a>
            </div> -->
            <ul class="mt-48">
                <li class="mt-8 border-r-8 border-purple-500">
                    <router-link to="/dashboard" class="block py-2 hover:bg-gray-200">
                        <img src="@/assets/img/dashboard.png" alt="" class="mx-auto">
                    </router-link>
                </li>
                <li class="mt-8">
                    <router-link to="/create" class="block py-2 hover:bg-gray-200">
                        <img src="@/assets/img/form.png" alt="" class="mx-auto">
                    </router-link>
                </li>
            </ul>
        </nav>
        <main class="block mr-auto" style="height: 100vh;">
            <nav class="w-full pt-12 ml-10">
                <h1 class="text-2xl text-gray-900 font-semibold">
                    Compliants and suggestions
                </h1>
                <form action="" class="mt-8">
                    <div class="relative">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-7 w-7 mt-1 absolute left-4 top-2 text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                        </svg>
                        <input type="text" class="pl-16 h-12 w-96 h-10 rounded-2xl outline-none shadow-2xl">
                    </div>
                </form>
            </nav>
            <div class="grid grid-cols-1 xl:grid-cols-3 gap-x-10 mx-10">
                <div v-for="complaint in  complaints" :key="complaint.id" class="rounded-3xl shadow-2xl mt-8 px-6 pt-10 pb-4" style=" background: url(./img/card.png) center no-repeat; background-size: cover;">
                    <h3 class="text-xl text-gray-900 font-medium ">
                        {{complaint.title}} 
                    </h3>
                    <h3 class="text-xl text-gray-600 font-medium mt-2">
                        Description: 
                    </h3>
                    <p class="text-lg text-gray-900 font-semibold">
                        {{complaint.description}}
                    </p>
                    <h3 class="text-xl text-gray-600 font-medium mt-2">
                        Location: 
                    </h3>
                    <p class="text-lg text-gray-900 font-semibold">
                        {{complaint.location}} 
                    </p>
                    <div class="flex mt-2">
                        <h3 class="text-xl text-gray-600 font-medium">
                            Votes: 
                        </h3>
                        <p class="text-xl text-gray-900 font-semibold ml-4">
                            {{complaint.voteCount}}
                        </p>
                    </div>
                    <div class="flex mt-2">
                        <h3 class="text-xl text-gray-600 font-medium">
                            Status: 
                        </h3>
                        <p class="text-xl text-gray-900 font-semibold ml-4">
                            {{complaint.status}}
                        </p>
                    </div>
                    <div v-if="votes[complaint.id]"  class="flex justify-center">
                        <button @click="removeVoteForComplaint(complaint.id)" class="inline-block bg-purple-900 hover:bg-purple-800 text-lg text-white text-center font-bold rounded-full px-4 py-2 mt-6">
                            Remove Vote
                        </button>
                    </div>
                    <div v-else class="flex justify-center">
                         <button @click="voteForComplaint(complaint.id)" class="inline-block bg-purple-500 hover:bg-purple-400 text-lg text-white text-center font-bold rounded-full px-4 py-2 mt-6">
                            Vote
                        </button>
                    </div>
                </div>
            </div>
        </main>
        <section class="px-10 border-l-4 border-blue-100">
            <div class="w-96 mt-12">
                <a type="button" @click="signOut" class="flex  items-center text-gray-900 bg-white hover:bg-gray-50 active:shadow-none py-2 rounded-2xl shadow-2xl text-xl text-center font-semibold flex item-center justify-center">
                    <img src="@/assets/img/near-logo.png" alt="near-logo">
                    <span >
                        {{accountId}}
                    </span> 
                    <div class="flex flex-col mx-3 items-center mt-1 ml-6">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" />
                              </svg>
                              <!-- <p class="text-sm">logout</p> -->
                        </div>
                </a>
            </div>
            <div class="w-full mt-24 flex justify-center">
                <router-link  to="/create" class="bg-purple-500 hover:bg-purple-400 text-white text-lg font-semibold py-4 px-8 rounded-full">New compliant</router-link>
            </div>
        </section>
    </header>
  </div>
</template>

<script>
import store from '../store/store.js'
import { useComplaints } from "@/composables/near"
import { wallet} from "@/services/near";
import router from '@/router/index.js'

export default {
    setup() {
        const accountId = store.state.accountId;
        const {  complaints, votes, voteForComplaint, removeVoteForComplaint, updateKey} = useComplaints();
        return {
            accountId,
            complaints,
            votes,
            updateKey,
            voteForComplaint,
            removeVoteForComplaint,
            signOut: () => {
                wallet.signOut();
                router.push('/')
            }
        }
    }
}
</script>