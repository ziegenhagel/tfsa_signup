
<template>
<q-page class="container">

    <q-stepper v-model="step" ref="stepper" color="primary" animated>

      <q-step :name="1" title="Programme"  :done="step > 1" >
        <h2>Programme</h2>
        <div class="card-wrapper" v-for="group in groups" :key="group.title">
            <h3>{{group.title}}</h3>
            <div class="row no-wrap">
                <q-card @click="setProgram(program)" v-for="program in group.programs" :key="program.title">
                    <q-card-section>
                        <i :class="program.icon || 'fa fa-file'"></i>
                        <h4>{{program.title}}</h4>
                    </q-card-section>
                </q-card>
            </div>
        </div>
      </q-step>
      
      <q-step :name="2" title="Programme" :done="step > 2" >
        <h2>Kurse</h2>
        <p>in {{program.title}}</p>
        <div class="card-wrapper" v-for="group in groups" :key="group.title">
            <h3>{{group.title}}</h3>
            <div class="row no-wrap">
                <q-card v-for="program in group.programs" :key="program.title">
                    <q-card-section>
                        <i :class="program.icon || 'fa fa-file'"></i>
                        <h4>{{program.title}}</h4>
                    </q-card-section>
                </q-card>
            </div>
        </div>
      </q-step>


      <template v-slot:navigation>
        <q-stepper-navigation>
          <q-btn @click="$refs.stepper.next()" color="primary" :label="step === 4 ? 'Finish' : 'Continue'" />
          <q-btn v-if="step > 1" flat color="primary" @click="$refs.stepper.previous()" label="Back" class="q-ml-sm" />
        </q-stepper-navigation>
      </template>

    </q-stepper>
</q-page>
</template>

<script>

import {ref} from 'vue'

export default {
    methods: {
        setProgram: (program )=>{
                        this.program = program 
            this.step++
        }
    },
    data: () => {
        return {
            step: ref(1),
            program: null,
            groups: [{
                    title: "Sommercamp",
                    programs: [{
                            title: "Jugendfilmcamp CLASSIC",
                            icon: "fa fa-user-graduate"
                        },
                        {
                            title: "Jugendfilmcamp STARTER",
                            icon: "fa fa-shapes"
                        },
                        {
                            title: "Medienklassenfahrten",
                            icon: "fa fa-school"
                        }
                    ]
                },
                {
                    title: "Events",
                    programs: [{
                        title: "Silvestercamp",
                        icon: "fa fa-glass-cheers"
                    }]
                },
                {
                    title: "Vermietung",
                    programs: [{
                        title: "Schnittraum",
                        icon: "fa fa-cut"
                    }]
                }
            ]
        }
    },
    name: 'Programs'
}
</script>

