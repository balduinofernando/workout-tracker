<template>
    <div class="max-w-screen-md mx-auto px-4 py-10">
        <!--Status Message-->
        <div v-if="statusMessage || errorMsg" class="mb-10 p-4 bg-light-grey rounded-md shadow-lg">
            <p class="text-at-light-green">{{ statusMessage }}</p>
            <p class="text-red-500">{{ errorMsg }}</p>
        </div>

        <!--Create-->
        <div class="p-8 flex items-start bg-light-grey rounded-md shadow-lg">
            <!--Form-->
            <form class="flex flex-col gap-y-5 w-full" @submit.prevent="createWorkout">
                <h1 class="text-2xl text-at-light-green">Adicione Treino</h1>

                <!--Workout Name-->
                <div class="flex flex-col">
                    <label for="workout-name" class="mb-1 text-sm text-at-light-green">Nome do Treino</label>
                    <input type="text" required class="p-2 rounded-sm text-gray-500 focus:outline-none"
                        v-model="workoutName" id="workout-name" />

                </div>

                <!--Workout Type-->
                <div class="flex flex-col">
                    <label for="workout-type" class="mb-1 text-sm text-at-light-green">Modelo de Treino</label>
                    <select id="workout-type" required class="p-2 rounded-sm bg-white text-gray-500 focus:outline-none"
                        v-model="workoutType" @change="workoutChange">
                        <option value="select-workout">Seleccione o Tipo de Treinamento</option>
                        <option value="strength">Treinamento de Força</option>
                        <option value="cardio">Cardio</option>
                    </select>
                </div>

                <!--Workout Types of Strength-->
                <div v-if="workoutType === 'strength'" class="flex flex-col gap-y-4">
                    <div v-for="(item, index) in exercises" :key="index"
                        class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row">

                        <!--Exercises-->
                        <div class="flex flex-col md:w-1/3">
                            <label for="exercise-name" class="mb-1 text-sm text-at-light-green">Exercício</label>
                            <input type="text" id="exercise-name" required
                                class="p-2 w-full text-gray-500 focus:outline-none" v-model="item.exercise">
                        </div>

                        <!--Sets-->
                        <div class="flex flex-col flex-1">
                            <label for="sets" class="mb-1 text-sm text-at-light-green">Conjuntos</label>
                            <input type="text" id="sets" required class="p-2 w-full text-gray-500 focus:outline-none"
                                v-model="item.sets">
                        </div>

                        <!-- Reps -->
                        <div class="flex flex-col flex-1">
                            <label for="reps" class="mb-1 text-sm text-at-light-green">Repetições</label>
                            <input type="text" id="reps" required class="p-2 w-full text-gray-500 focus:outline-none"
                                v-model="item.reps">
                        </div>

                        <!-- Weight -->
                        <div class="flex flex-col flex-1">
                            <label for="weight" class="mb-1 text-sm text-at-light-green">Peso (em Kg)</label>
                            <input type="text" id="weight" required class="p-2 w-full text-gray-500 focus:outline-none"
                                v-model="item.weight">
                        </div>

                        <img src="@/assets/images/trash-light-green.png" @click="deleteExercise(item.id)"
                            class="h-4 w-auto absolute -left-5 cursor-pointer" alt="Trash Icon" />


                    </div>
                    <button type="button" @click="addExercise"
                        class="mt-6 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green">Adicionar
                    </button>
                </div>

                <!-- Workout Types Of Cardio-->
                <div v-if="workoutType === 'cardio'" class="flex flex-col gap-y-4">
                    <div v-for="(item, index) in exercises" :key="index"
                        class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row">

                        <!-- Exercises -->
                        <div class="flex flex-col md:w-1/3">
                            <label for="cardio-type" class="mb-1 text-sm text-at-light-green">Tipo</label>
                            <select id="cardio-type" class="p-2 w-full bg-white text-gray-500 focus:outline-none"
                                v-model="item.cardioType">
                                <option value="#" selected>Seleccione</option>
                                <option value="run">Corrida</option>
                                <option value="walk">Marcha</option>
                            </select>
                        </div>

                        <!-- Distância -->
                        <div class="flex flex-col flex-1">
                            <label for="distance" class="mb-1 text-sm text-at-light-green">Distância</label>
                            <input type="text" id="distance" required class="p-2 w-full text-gray-500 focus:outline-none"
                                v-model="item.distance">
                        </div>

                        <!-- Duração -->
                        <div class="flex flex-col flex-1">
                            <label for="duration" class="mb-1 text-sm text-at-light-green">Duração</label>
                            <input type="text" id="duration" required class="p-2 w-full text-gray-500 focus:outline-none"
                                v-model="item.duration">
                        </div>

                        <!-- Pace -->
                        <div class="flex flex-col flex-1">
                            <label for="pace" class="mb-1 text-sm text-at-light-green">Ritmo</label>
                            <input type="text" id="pace" required class="p-2 w-full text-gray-500 focus:outline-none"
                                v-model="item.pace">
                        </div>

                        <img @click="deleteExercise(item.id)" src="@/assets/images/trash-light-green.png"
                            class="h-4 w-auto absolute -left-5 cursor-pointer" alt="Trash Icon" />


                    </div>
                    <button type="button" @click="addExercise"
                        class="mt-6 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green">Adicionar</button>
                </div>

                <button type="submit" v-if="workoutType !== 'select-workout'"
                    class="mt-6 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green">
                    Gravar Treino</button>
            </form>
        </div>
    </div>
</template>
<script setup>
import { ref } from 'vue'
import { uid } from 'uid'
import { supabase } from '../supabase/init'

/*Create Data */
const workoutName = ref('');
const workoutType = ref("select-workout");
const exercises = ref([]);
const statusMessage = ref(null);
const errorMsg = ref(null);

// Add Exercise
const addExercise = () => {
    if (workoutType === 'strength') {
        exercises.value.push({
            id: uid(),
            exercise: '',
            sets: '',
            reps: '',
            weight: ''
        });

        return;
    }

    exercises.value.push({
        id: uid(),
        cardioType: '',
        distance: '',
        duration: '',
        pace: ''
    })

}

/* Delete Exercise */
const deleteExercise = (id) => {
    if (exercises.value.length > 1) {
        exercises.value = exercises.value.filter(exercise => exercise.id !== id)
        return;
    }

    errorMsg.value = "Erro: Não Pode remover todos, precisa pelo menos um exercício";
    setTimeout(() => {
        errorMsg.value = false;
    }, 5000)
}

// Listem for changing of workout type input

const workoutChange = () => {
    exercises.value = [];
    addExercise();
}

// Create Workout
const createWorkout = async () => {

    try {
        const { data, error } = await supabase.from('workouts').insert([
            {
                workoutName: workoutName.value,
                workoutType: workoutType.value,
                exercises: exercises.value,
            },
        ]);

        if (error) throw error;

        // Flash Message
        statusMessage.value = "Sucesso: Treino Adicionado";

        // Reset Fields
        workoutName.value = null;
        workoutType.value = 'select-workout';
        exercises.value = [];

        // Clear Flash Message
        setTimeout(() => {
            statusMessage.value = false;
        }, 5000);

    } catch (error) {
        errorMsg.value = `Erro: ${error.message}`;

        setTimeout(() => {
            errorMsg.value = false;
        }, 5000);
    }

}

</script>