<script setup>
import Card from '@/Components/Card.vue';
import FormInput from '@/Components/FormInput.vue';
import FormSelect from '@/Components/FormSelect.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import MidwifeLayout from '@/Layouts/MidwifeLayout.vue';
import { router, useForm, usePage } from '@inertiajs/vue3';
import SecondaryButton from '@/Components/SecondaryButton.vue';
import { ref } from 'vue';
import { useNow, useDateFormat } from '@vueuse/core'
import FormTextarea from '@/Components/FormTextarea.vue';

const props = defineProps({
    checkUp: Object
})

const page = usePage();
const today = useDateFormat(useNow(), 'YYYY-MM-DD HH:mm:ss')

let form = useForm({
    weight: '',
    height: '',
    age_of_gestation: '',
    blood_pressure: '',
    body_mass_index: '',
    laboratory_test_done: '',
    urinalysis: '',
    complete_blood_count: '',
    blood_typing: '',
    advice_and_services: '',
    name_of_service_worker: page.props.auth.user.name,
    done_at: props.checkUp.done_at ?? today.value,
})

const submitHandler = () => {
    form.patch(route('midwife.schedules.update', { checkup: props.checkUp.id }))
}



</script>

<template>
    <MidwifeLayout title="Check Up Form">
        <div>
            <form @submit.prevent="submitHandler">
                <Card>
                    <div class="sm:col-span-3">
                        <FormInput v-model="form.weight" :error="form.errors.weight" type="number"
                            label="Weight in Kilogram" />
                    </div>
                    <div class="sm:col-span-3">
                        <FormInput v-model="form.height" :error="form.errors.height" type="number"
                            label="Heigh in Centimeter" />
                    </div>
                    <div class="sm:col-span-3">
                        <FormInput v-model="form.age_of_gestation" :error="form.errors.age_of_gestation" type="number"
                            label="Age of gestation" />
                    </div>
                    <div class="sm:col-span-3">
                        <FormInput v-model="form.blood_pressure" :error="form.errors.blood_pressure" type="text"
                            label="Blood Pressure" />
                    </div>
                    <div class="sm:col-span-3">
                        <FormInput v-model="form.blood_typing" :error="form.errors.blood_typing" type="text"
                            label="Body Mass Index" />
                    </div>
                    <div class="sm:col-span-3">
                        <FormInput v-model="form.complete_blood_count" :error="form.errors.complete_blood_count"
                            type="number" label="Complete Blood Count" />
                    </div>
                    <div class="sm:col-span-3">
                        <FormInput v-model="form.body_mass_index" :error="form.errors.body_mass_index" type="text"
                            label="Blood Typing" />
                    </div>
                    <div class="sm:col-span-6">
                        <FormTextarea v-model="form.laboratory_test_done" :error="form.errors.laboratory_test_done"
                            label="Laboratory Test Done" />
                    </div>
                    <div class="sm:col-span-6">
                        <FormTextarea v-model="form.urinalysis" :error="form.errors.urinalysis" label="Urinalysis" />
                    </div>
                    <div class="sm:col-span-6">
                        <FormTextarea v-model="form.advice_and_services" :error="form.errors.advice_and_services"
                            label="Advice and Services" />
                    </div>
                    <div class="sm:col-span-3">
                        <FormInput v-model="form.name_of_service_worker" :error="form.errors.name_of_service_worker"
                            type="text" label="Name of Service worker" />
                    </div>
                    <template #footer>
                        <div class="flex space-x-3">
                            <PrimaryButton :disabled="form.processing" :loading-on="form.processing">Save Update
                            </PrimaryButton>
                        </div>
                    </template>
                </Card>
            </form>
        </div>
    </MidwifeLayout>
</template>