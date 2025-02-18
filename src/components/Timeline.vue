<script setup lang="ts">
    import { ref, computed } from "vue"
    import type { TimelinePost } from "../posts"
    import { today, thisWeek, thisMonth } from "../posts"
    import { DateTime } from "luxon"  
    import TimelineItem from './TimelineItem.vue'  

    const periods = ["Aujourd'hui", "Cette semaine", "Ce mois"] as const
    type Period = typeof periods[number]

    let selectedPeriod = ref("Aujourd'hui")

    function selectPeriod(period : Period): void{
        selectedPeriod.value = period
        console.log(period)
    }

    const posts = computed<TimelinePost[]>(() => {
        return [today, thisWeek, thisMonth].map(post => {
            return{
                ...post,
                created: DateTime.fromISO(post.created)
            }
        }).filter(post=>{
        if(selectedPeriod.value === "Aujourd'hui"){
            return post.created >= DateTime.now().minus({day:1})
        }
        if (selectedPeriod.value === "Cette semaine") {
            return post.created >= DateTime.now().minus({ week: 1 })
        }
        if(selectedPeriod.value === "Ce mois"){
            return post.created >= DateTime.now().minus({month:1})
        }
        return post
    })
})

</script>

<template>
    Timeline
    <nav>
        <span>
            <a href="#" v-for="period of periods" v-bind:key="period" v-on:click="selectPeriod(period)" v-bind:class="{'bg-primary': period === selectedPeriod}">
                {{ period }}
            </a>
        </span>
    </nav>
    <br>
    {{ selectedPeriod }}
    <br>
    <br>
    <div class="btn-group" role="group">
        <button type="button" class="btn btn-primary" v-for="period of periods" v-bind:key="period" v-on:click="selectPeriod(period)" v-bind:class="{ 'btn-success': period === selectedPeriod}">
            {{ period }}
        </button>
    </div>
    <h2>{{ selectedPeriod }}</h2>
    <ul>
        <TimelineItem v-for="post of posts" :key="post.id" :post="post"/>
    </ul>
    <br>
</template>

<style>

</style>