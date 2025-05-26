<template>
  <div class="min-h-screen bg-gray-50">
    <!-- Header -->
    <header class="bg-white shadow-sm">
      <div class="max-w-6xl mx-auto px-4 py-4">
        <div class="flex items-center justify-between">
          <h1 class="text-2xl font-bold text-blue-600">Skinaria</h1>
          <nav class="flex space-x-8">
            <a href="#" class="text-gray-700 hover:text-blue-600">Beranda</a>
            <a href="#" class="text-gray-700 hover:text-blue-600">Tentang</a>
            <a href="#" class="text-gray-700 hover:text-blue-600">Cara Kerja</a>
            <a href="#" class="text-gray-700 hover:text-blue-600">Artikel</a>
          </nav>
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-6xl mx-auto px-4 py-8">
      <!-- Breadcrumb -->
      <div class="mb-8">
        <span class="text-gray-500">Beranda</span>
        <span class="text-gray-400 mx-2">/</span>
        <span class="text-blue-600">Unggah Gambar</span>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-start">
        <!-- Upload Section -->
        <div>
          <h2 class="text-3xl font-bold text-gray-900 mb-4">Unggah Gambar</h2>
          <p class="text-gray-600 mb-8">Tambahkan gambar anda di sini</p>

          <!-- Upload Area -->
          <div
            class="border-2 border-dashed rounded-lg p-12 text-center transition-all duration-200"
            :class="
              dragOver
                ? 'border-blue-500 bg-blue-50'
                : 'border-gray-300 bg-white'
            "
            @dragover.prevent="handleDragOver"
            @dragleave.prevent="handleDragLeave"
            @drop.prevent="handleDrop"
          >
            <div v-if="!selectedFile" class="space-y-4">
              <!-- Upload Icon -->
              <div class="flex justify-center">
                <div
                  class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center"
                >
                  <svg
                    class="w-6 h-6 text-blue-600"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12"
                    />
                  </svg>
                </div>
              </div>

              <div>
                <p class="text-gray-700 font-medium mb-2">
                  Drag your file(s) to start uploading
                </p>
                <p class="text-gray-500 text-sm mb-4">OR</p>
                <button
                  @click="handleBrowseClick"
                  class="px-6 py-2 border border-blue-500 text-blue-600 rounded-lg hover:bg-blue-50 transition-colors"
                >
                  Browse files
                </button>
              </div>
            </div>

            <!-- File Preview -->
            <div v-else class="space-y-4">
              <div class="flex justify-center">
                <img
                  :src="filePreview"
                  :alt="selectedFile.name"
                  class="max-w-48 max-h-48 object-cover rounded-lg shadow-md"
                />
              </div>
              <p class="text-gray-700 font-medium">{{ selectedFile.name }}</p>
              <p class="text-sm text-gray-500">
                {{ formatFileSize(selectedFile.size) }}
              </p>

              <!-- Upload Progress -->
              <div
                v-if="uploadProgress > 0 && uploadProgress < 100"
                class="w-full"
              >
                <div class="w-full bg-gray-200 rounded-full h-2">
                  <div
                    class="bg-blue-600 h-2 rounded-full transition-all duration-300"
                    :style="`width: ${uploadProgress}%`"
                  ></div>
                </div>
                <p class="text-sm text-gray-600 mt-2">
                  Uploading... {{ uploadProgress }}%
                </p>
              </div>

              <!-- Success State -->
              <div v-if="uploadProgress === 100" class="text-green-600">
                <svg
                  class="w-6 h-6 mx-auto mb-2"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 13l4 4L19 7"
                  />
                </svg>
                <p class="text-sm">Upload berhasil!</p>
              </div>

              <button
                @click="resetUpload"
                class="text-sm text-gray-500 hover:text-gray-700 underline"
              >
                Ganti gambar
              </button>
            </div>

            <!-- Hidden file input -->
            <input
              ref="fileInput"
              type="file"
              accept="image/*"
              class="hidden"
              @change="handleFileInputChange"
            />
          </div>
        </div>

        <!-- Instructions Section -->
        <div>
          <h3 class="text-xl font-semibold text-gray-900 mb-6">Instruksi</h3>
          <div class="space-y-4">
            <div class="flex items-start space-x-3">
              <div
                class="w-6 h-6 bg-blue-100 text-blue-600 rounded-full flex items-center justify-center flex-shrink-0 text-sm font-semibold mt-0.5"
              >
                1
              </div>
              <p class="text-gray-700">
                Pastikan gambar yang akan diunggah tidak blur.
              </p>
            </div>

            <div class="flex items-start space-x-3">
              <div
                class="w-6 h-6 bg-blue-100 text-blue-600 rounded-full flex items-center justify-center flex-shrink-0 text-sm font-semibold mt-0.5"
              >
                2
              </div>
              <p class="text-gray-700">
                Gambar harus memiliki pencahayaan yang cukup.
              </p>
            </div>

            <div class="flex items-start space-x-3">
              <div
                class="w-6 h-6 bg-blue-100 text-blue-600 rounded-full flex items-center justify-center flex-shrink-0 text-sm font-semibold mt-0.5"
              >
                3
              </div>
              <p class="text-gray-700">
                Pastikan objek gambar terlihat dengan jelas.
              </p>
            </div>
          </div>

          <!-- Check Skin Condition Button -->
          <button
            @click="handleCheckSkinCondition"
            :disabled="!selectedFile || uploadProgress < 100"
            class="mt-8 w-full bg-blue-600 text-white py-3 px-6 rounded-lg font-medium hover:bg-blue-700 disabled:bg-gray-300 disabled:cursor-not-allowed transition-colors flex items-center justify-center space-x-2"
          >
            <span>Cek Kondisi Kulitmu</span>
            <svg
              class="w-5 h-5"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M13 7l5 5m0 0l-5 5m5-5H6"
              />
            </svg>
          </button>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

// Reactive variables
const dragOver = ref(false);
const selectedFile = ref(null);
const uploadProgress = ref(0);
const fileInput = ref(null);

// Computed properties
const filePreview = computed(() => {
  if (selectedFile.value) {
    return URL.createObjectURL(selectedFile.value);
  }
  return null;
});

// Methods
const handleDragOver = (e) => {
  e.preventDefault();
  dragOver.value = true;
};

const handleDragLeave = (e) => {
  e.preventDefault();
  dragOver.value = false;
};

const handleDrop = (e) => {
  e.preventDefault();
  dragOver.value = false;

  const files = e.dataTransfer.files;
  if (files.length > 0) {
    handleFileSelect(files[0]);
  }
};

const handleFileSelect = (file) => {
  if (file && file.type.startsWith("image/")) {
    selectedFile.value = file;
    simulateUpload();
  } else {
    alert("Harap pilih file gambar yang valid");
  }
};

const simulateUpload = () => {
  uploadProgress.value = 0;
  const interval = setInterval(() => {
    uploadProgress.value += 10;
    if (uploadProgress.value >= 100) {
      clearInterval(interval);
    }
  }, 200);
};

const handleBrowseClick = () => {
  fileInput.value?.click();
};

const handleFileInputChange = (e) => {
  const file = e.target.files[0];
  if (file) {
    handleFileSelect(file);
  }
};

const handleCheckSkinCondition = () => {
  if (selectedFile.value && uploadProgress.value === 100) {
    alert("Memulai analisis kondisi kulit...");
    // Di sini Anda bisa navigate ke halaman hasil atau memulai analisis
    // Contoh: router.push('/hasil-analisis')
  } else {
    alert("Silakan unggah gambar terlebih dahulu");
  }
};

const resetUpload = () => {
  selectedFile.value = null;
  uploadProgress.value = 0;
  if (fileInput.value) {
    fileInput.value.value = "";
  }
};

const formatFileSize = (bytes) => {
  if (bytes === 0) return "0 Bytes";
  const k = 1024;
  const sizes = ["Bytes", "KB", "MB", "GB"];
  const i = Math.floor(Math.log(bytes) / Math.log(k));
  return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + " " + sizes[i];
};
</script>
