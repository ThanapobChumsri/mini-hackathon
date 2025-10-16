<template>
    <div class="min-h-screen bg-[#f5f6fb] flex items-center justify-center py-16 px-4">
        <Transition name="fade" mode="out-in">
            <div v-if="step === 'form'" key="form"
                class="w-full max-w-3xl rounded-[32px] border border-[#e7e9f2] bg-white p-10 shadow-[0_24px_60px_rgba(15,23,42,0.08)]">
                <div
                    class="mb-8 flex max-w-md items-center gap-5 rounded-[24px] border border-[#dfe4f3] bg-white py-5 pl-8 pr-10 shadow-[0_16px_30px_rgba(15,23,42,0.06)]">
                    <div class="text-2xl font-semibold text-slate-600">Jobs</div>
                    <button type="button"
                        class="ml-auto rounded-full bg-[#4b7ee8] px-6 py-2 text-white text-sm font-semibold shadow-[0_12px_24px_rgba(75,126,232,0.35)] transition hover:bg-[#3f6fd4]">
                        + New job
                    </button>
                </div>

                <section class="space-y-6 rounded-[28px] border border-[#e2e8f5] bg-white/80 p-8 shadow-inner">
                    <header class="space-y-2">
                        <h2 class="text-2xl font-semibold text-slate-700">New job</h2>
                        <p class="text-sm leading-6 text-slate-500">
                            Enter a job description for the role. If you don’t have one, briefly describe the role
                            and we will assist you.
                        </p>
                    </header>

                    <UForm :state="form" class="space-y-6">
                        <UFormGroup label="Job position" name="position" required>
                            <UInput v-model="form.position" size="xl" placeholder="Senior Back-End Developer"
                                class="rounded-2xl" />
                        </UFormGroup>
                        <UFormGroup label="Job description" name="description">
                            <UTextarea v-model="form.description" rows="4" placeholder="Paste your job description here"
                                class="rounded-2xl" />
                        </UFormGroup>
                    </UForm>

                    <p v-if="errorMessage" class="text-sm text-red-500">{{ errorMessage }}</p>

                    <div class="flex items-center justify-end gap-4">
                        <button type="button"
                            class="rounded-full px-6 py-2 text-sm font-semibold text-slate-400 transition hover:text-slate-600"
                            @click="resetForm">
                            Cancel
                        </button>
                        <button type="button"
                            class="flex items-center gap-2 rounded-full bg-[#4b7ee8] px-7 py-2 text-sm font-semibold text-white shadow-[0_12px_30px_rgba(75,126,232,0.4)] transition hover:bg-[#3f6fd4]"
                            @click="handleCreate">
                            <span class="inline-block h-2 w-2 rounded-full bg-white"></span>
                            Create
                        </button>
                    </div>
                </section>
            </div>

            <div v-else key="criteria"
                class="w-full max-w-4xl rounded-[32px] border border-[#e7e9f2] bg-white p-8 shadow-[0_24px_60px_rgba(15,23,42,0.08)]">
                <header class="flex flex-wrap items-center justify-between gap-6 border-b border-[#dfe4f3] pb-6">
                    <div>
                        <p class="text-lg font-semibold text-slate-500">
                            Job Position:
                            <span class="font-normal italic text-slate-400">{{ form.position }}</span>
                        </p>
                    </div>
                    <nav class="flex items-center gap-3 text-sm font-medium">
                        <button class="rounded-full border border-[#e2e8f5] px-5 py-2 text-[#7a87a6] hover:bg-[#f7f9ff]">
                            Description
                        </button>
                        <button
                            class="rounded-full bg-[#4b7ee8] px-5 py-2 text-white shadow-[0_12px_24px_rgba(75,126,232,0.35)]">
                            Criteria
                        </button>

                    </nav>
                </header>

                <section class="mt-8 space-y-8">
                    <div v-for="item in criteria" :key="item.id"
                        class="rounded-[24px] border border-[#e2e8f5] bg-[#f8f9ff] p-6">
                        <header class="flex flex-wrap items-center justify-between gap-4 pb-4">
                            <div>
                                <h3 class="text-lg font-semibold text-[#42527b]">{{ item.title }}</h3>
                                <p v-if="item.description" class="text-sm text-slate-400">{{ item.description }}</p>
                            </div>
                            <div class="flex items-center gap-2 text-xs font-semibold text-[#42527b]">
                                Mandatory
                                <UToggle v-model="item.mandatory" color="primary" />
                            </div>
                        </header>

                        <div class="space-y-4">
                            <UInput v-model="item.value" :placeholder="item.placeholder"
                                class="rounded-2xl border-[#dbe2f3] bg-white" />
                            <UButton variant="soft" color="primary"
                                class="rounded-full border border-[#cdd9fb] bg-[#e1e8ff] text-[#4b7ee8] hover:bg-[#d3dcff]">
                                Add another
                            </UButton>
                        </div>
                    </div>





                </section>

                <div class="flex justify-end mt-4">
                    <button @click="step = 'form'"
                        class="rounded-full bg-[#4b7ee8] px-5 py-2 text-white shadow-[0_12px_24px_rgba(75,126,232,0.35)] ">
                        Cancel
                    </button>
                </div>
            </div>
        </Transition>
    </div>
</template>

<script setup>
const step = ref("form")
const errorMessage = ref("")

const form = reactive({
    position: "",
    description: "",
})

const criteria = reactive([
    {
        id: "workingExperience",
        title: "Working experience",
        description: "",
        placeholder: "Accomplished developer with a minimum of 7 years of experience",
        mandatory: true,
        value: "",
    },
    {
        id: "skills",
        title: "Skills",
        description: "",
        placeholder: "Proficiency in TypeScript, with a minimum of 2 years of experience",
        mandatory: true,
        value: "",
    },
    {
        id: "education",
        title: "Education",
        description: "",
        placeholder: "Bachelor's degree in computer science or software engineering",
        mandatory: false,
        value: "",
    },
    {
        id: "additional",
        title: "Additional",
        description: "",
        placeholder: "Based in USA",
        mandatory: true,
        value: "",
    },
])

const resetForm = () => {
    form.position = ""
    form.description = ""
    errorMessage.value = ""
}

const handleCreate = () => {
    if (!form.position.trim()) {
        errorMessage.value = "Please provide a job position before continuing."
        return
    }

    errorMessage.value = ""
    step.value = "criteria"
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.25s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>
