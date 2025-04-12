<template>
  <div class="grid grid-cols-2 gap-4">
    <!-- Preview Section -->
    <div class="border border-gray-200 rounded-lg overflow-hidden">
      <div class="p-4 bg-gray-50 border-b border-gray-200">
        <h3 class="text-lg font-medium">Résultat</h3>
      </div>
      <div class="p-4">
        <div ref="previewElement" class="min-h-[200px]"></div>
      </div>
    </div>

    <!-- Editor Section -->
    <div class="border border-gray-200 rounded-lg overflow-hidden">
      <div class="p-4 bg-gray-50 border-b border-gray-200 flex justify-between items-center">
        <h3 class="text-lg font-medium">Code</h3>
        <button 
          @click="runCode" 
          class="px-3 py-1 bg-blue-500 text-white rounded-md text-sm hover:bg-blue-600 transition-colors focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:ring-offset-gray-900"
        >
          Run
        </button>
      </div>
      <div class="p-4 bg-gray-900">
        <textarea
          v-model="editorCodeHtml"
          class="w-full h-[200px] bg-gray-900 text-gray-100 font-mono text-sm p-4 border border-gray-700 rounded-lg resize-none focus:outline-none focus:border-blue-500 focus:ring-2 focus:ring-blue-500 mb-4 shadow-inner hover:border-gray-600 transition-colors"
          @keydown.ctrl.enter="runCode"
          @keydown.meta.enter="runCode"
          placeholder="Enter HTML code here..."
        ></textarea>
        <textarea
          v-model="editorCodeJs"
          class="w-full h-[200px] bg-gray-900 text-gray-100 font-mono text-sm p-4 border border-gray-700 rounded-lg resize-none focus:outline-none focus:border-blue-500 focus:ring-2 focus:ring-blue-500 shadow-inner hover:border-gray-600 transition-colors"
          @keydown.ctrl.enter="runCode"
          @keydown.meta.enter="runCode"
          placeholder="Enter JavaScript code here..."
        ></textarea>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const previewElement = ref(null)
const editorCodeHtml = ref(`
  <div id="test-box" class="flex justify-center items-center border-2 rounded w-40 h-40 bg-blue-100">
    Test d'interaction
  </div>
`)

const editorCodeJs = ref(`
  const box = document.getElementById('test-box')
  box.addEventListener('mouseover', () => {
    box.style.backgroundColor = 'rgb(254, 202, 202)'
    box.textContent = 'Souris dessus !'
  })
  box.addEventListener('mouseout', () => {
    box.style.backgroundColor = 'rgb(219, 234, 254)'
    box.textContent = "Test d'interaction"
  })
`)

const runCode = () => {
  if (!previewElement.value) return
  
  try {
    // Set the HTML content
    previewElement.value.innerHTML = editorCodeHtml.value
    
    // Create and append the script
    const script = document.createElement('script')
    script.textContent = editorCodeJs.value
    previewElement.value.appendChild(script);
  } catch (error) {
    console.error('Error evaluating code:', error)
    previewElement.value.textContent = `Error: ${error.message}`
  }
}

onMounted(() => {
  runCode()
})
</script>
