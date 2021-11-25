<template>
<q-page>
    <q-stepper alternative-labels v-model="step" ref="stepper" color="primary" animated>

        <q-step :name="1" title="Programm" :done="step > 1">
            <h2>Programm wählen</h2>
            <div class="card-wrapper" v-for="group in groups" :key="group.title">
                <h3>{{group.title}}</h3>
                <div class="row no-wrap">
                    <q-btn @click="setProgram(program)" v-for="program in group.programs" :key="program.title" push>
                        <i :class="program.icon || 'fa fa-file'"></i>
                        <h4>{{program.title}}</h4>
                    </q-btn>
                </div>
            </div>
        </q-step>

        <q-step :name="2" title="Kurs" :done="step > 2">
            <h2><i :class="program.icon"></i> Kurse</h2>
            <p>in {{program.title}}</p>
            <q-list v-if="program.courses && program.courses.length > 0">
                <q-item @click="setCourse(course)" v-for="course in program.courses" :key="course.title" clickable v-ripple>
                    <q-item-section top avatar>
                        <q-avatar :icon="course.icon" color="blue-grey-2"></q-avatar>
                    </q-item-section>

                    <q-item-section>
                        <q-item-label>{{course.title}}</q-item-label>
                        <q-item-label caption>{{course.description}}</q-item-label>
                    </q-item-section>

                    <q-item-section side center>
                        <i class="fa fa-chevron-right"></i>
                    </q-item-section>
                </q-item>
            </q-list>
            <q-banner inline-actions rounded class="bg-orange text-white" v-else>
                Aktuell keine Kurse in {{program.title}} verfügbar.
            </q-banner>
        </q-step>

        <q-step :name="3" title="Termin" :done="step > 3">
            <h2><i :class="program.icon"></i> Termine</h2>
            <p>in {{program.title}} / {{course.title}}</p>

            <q-list v-if="course.events && course.events.length > 0">
                <q-item @click="setEvent(event)" v-for="(event,index) in course.events" :key="index" clickable v-ripple>
                    <q-item-section top avatar>
                        <q-avatar icon="event" color="blue-grey-2"></q-avatar>
                    </q-item-section>

                    <q-item-section>
                        <q-item-label>{{event.title}}</q-item-label>
                        <q-item-label caption>{{event.start}} - {{event.end}}</q-item-label>
                        <q-badge :color="event.status">Plätze verfügbar</q-badge>
                    </q-item-section>

                    <q-item-section side center>
                        <i class="fa fa-chevron-right"></i>
                    </q-item-section>
                </q-item>
            </q-list>

            <q-banner inline-actions rounded class="bg-orange text-white" v-else>
                Aktuell keine Termine in {{program.title}} / {{course.title}} verfügbar.
            </q-banner>
        </q-step>

        <q-step :name="4" title="Daten" :done="step > 4">
            <h2><i class="fa fa-user"></i> Daten eingeben</h2>
            <p>in {{program.title}} / {{course.title}} / {{event.title}} </p>
            <q-form>

                <q-input outlined label="Geburtstag">
                    <template v-slot:prepend>
                        <q-icon name="event" />
                    </template>
                </q-input>
                <q-input outlined label="Noch etwas" />

            </q-form>
        </q-step>

        <q-step :name="5" title="Fertig" :done="step > 5">
            <h1>Vielen Dank! </h1>
            <p>Du hast eine Bestätigungsmail an deine Email Adresse gesendet bekommen.</p>
            <p>Dort findset Du Details zur weiteren Vorgehensweise.</p>
            <q-btn-group>
                <q-btn color="white" text-color="black" push @click="reset()">Weitere Anmeldung</q-btn>
                <q-btn color="primary" push>Anmeldeseite schliessen</q-btn>
            </q-btn-group>
        </q-step>

        <template v-slot:navigation v-if="step < 5">
            <q-stepper-navigation v-if="step > 1">
                <q-btn v-if="step > 1" color="white" text-color="black" @click="$refs.stepper.previous()" label="Zurück" push />
                <q-btn v-if="step > 3" @click="$refs.stepper.next()" color="primary" label="Anmeldung abschliessen" push />
            </q-stepper-navigation>
        </template>

    </q-stepper>
</q-page>
</template>

<script lang="ts">
interface Program {
    title: string, icon: string, courses: Course[]
}
interface Course {
    title: string, icon: string, description ? : string, events: Event[]
}
interface Event {
    title: string, start: string, end: string, status: "red" | "green" | "orange"
}

import {
    ref,
    defineComponent
} from 'vue';

export default defineComponent({
    methods: {
        setProgram: function (program: Program) {
            this.program = program
            this.step++
        },
        setCourse: function (course: Course) {
            this.course = course
            this.step++
        },
        setEvent: function (event: Event) {
            this.event = event
            this.step++
        },
        reset: function() {this.step = 1}
    },
    data: () => {
        return {
            step: ref(1),
            program: null as Program,
            course: null as Course,
            event: null as Event,
            groups: [{
                    title: "Jugendfilmcamp ",
                    programs: [{
                            title: "CLASSIC",
                            icon: "fa fa-user-graduate",
                            courses: [{
                                    title: "Kamera Workshop",
                                    icon: "fa fa-camera",
                                    description: "Lerne mit der Kamera umzugehen",
                                    events: [{
                                        title: "Woche 1",
                                        start: "01.12.2021",
                                        end: "08.12.2021",
                                        status: "green"
                                    }] as Event[]
                                },
                                {
                                    title: "Schauspiel Workshop",
                                    icon: "fa fa-theater-masks",
                                    description: "Lerne mit der Kamera umzugehen"
                                }
                            ] as Course[]
                        },
                        {
                            title: "STARTER",
                            icon: "fa fa-shapes",
                            courses: [{
                                    title: "Kamera Workshop",
                                    icon: "fa fa-camera",
                                    description: "Lerne mit der Kamera umzugehen"
                                },
                                {
                                    title: "Schauspiel Workshop",
                                    icon: "fa fa-theater-masks",
                                    description: "Lerne mit der Kamera umzugehen"
                                }
                            ]
                        }
                    ] as Program[]
                },
                {
                    title: "Bildung",
                    programs: [{
                        title: "Medienklassenfahrten",
                        icon: "fa fa-school"
                    }]
                },
                {
                    title: "Events & Vermietung",
                    programs: [{
                            title: "Silvestercamp",
                            icon: "fa fa-glass-cheers"
                        },
                        {
                            title: "Schnittraum",
                            icon: "fa fa-cut"
                        }
                    ]
                }
            ]
        }
    },
    name: 'Programs'
})
</script>
