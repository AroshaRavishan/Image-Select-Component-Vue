<script setup>
import {
    ref
} from "vue";
import {
    router
} from "@inertiajs/vue3";

const photoPreview = ref(null);
const photoInput = ref(null);

const props = defineProps({
    photoPath: {
        type: String,
        default: ''
    },
    photoAlt: {
        type: String,
        default: ''
    },
    label: {
        type: String,
        default: 'Photo'
    },
    deletePhotoRoute: {
        type: String,
        default: 'photo.destroy'
    },
    isEnableRemove: {
        type: Boolean,
        default: false
    },
    isEnableDelete: {
        type: Boolean,
        default: false
    },
})

const emit = defineEmits(['update:modelValue'])

const updatePhotoPreview = () => {
    const photo = photoInput.value.files[0];
    emit('update:modelValue', photoInput.value.files[0])

    if (!photo) return;

    const reader = new FileReader();

    reader.onload = (e) => {
        photoPreview.value = e.target.result;
    };

    reader.readAsDataURL(photo);
};

const selectNewPhoto = () => {
    photoInput.value.click();
};

const deletePhoto = () => {
    router.delete(route(props.deletePhotoRoute), {
        preserveScroll: true,
        onSuccess: () => {
            photoPreview.value = null;
            clearPhotoFileInput();
        },
    });
};

const clearPhotoFileInput = () => {
    if (photoInput.value?.value) {
        photoInput.value.value = null;
    }
};

const removeFile = () => {
    emit('update:model-value', null);
    photoPreview.value = null;
};
</script>

<template>
    <div class="flex items-center gap-5">
        <div class="flex justify-center lg:justify-start">
            <!-- Current Profile Photo -->
            <div v-show="!photoPreview" class="">
                <img loading="lazy" :src="photoPath" :alt="photoAlt" class="h-20 w-20 object-cover rounded-full border border-ash-350 bg-ash-350">
            </div>

            <!-- New Profile Photo Preview -->
            <div v-show="photoPreview" class="">
                <span class="block w-20 h-20 bg-cover bg-no-repeat bg-center rounded-full border border-ash-350 bg-ash-350" :style="'background-image: url(\'' + photoPreview + '\');'" />
            </div>
        </div>
        <div class="grid">
            <input ref="photoInput" type="file" class="hidden" @change="updatePhotoPreview" accept=".jpg, .jpeg, .png">

            <InputLabel :for="label" :value="label" customClass="inline-block mb-2 ml-0 font-normal text-sm text-ash-450 dark:text-white/80" />
            <span class="font-normal text-sm text-ash-450">&nbsp;Only jpg, Jpeg and png files are allowed</span>
            <Button class="mt-2 mr-2 bg-white rounded-1.5 w-fit py-1.5 px-7" type="button" @click.prevent="selectNewPhoto">
                Select an Image
            </Button>

            <Button v-if="photoPath && isEnableRemove" type="button" class="mt-2" @click.prevent="deletePhoto">
                Remove Photo
            </Button>
            <button v-if="photoPreview && isEnableDelete" class="mt-2 font-light text-sm text-base font-semibold hover:text-red-600 text-gray-400 border hover:border-red-600 rounded-lg border-gray-400 px-7 py-2 cursor-pointer mr-4 leading-pro hover:bg-red-100" @click.prevent="removeFile">
                <i class="fas fa-trash"></i>
            </button>
        </div>
    </div>
</template>

<style scoped>
.font-size-hint {
    font-size: 13px;
}
</style>