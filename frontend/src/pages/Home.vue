<template>
  <div class="m-5">
    <!-- Heading and Create Button -->
    <div class="flex items-baseline justify-between mb-4">
      <h2 class="text-gray-900 font-semibold text-xl">Links</h2>
      <Button
        variant="solid"
        theme="gray"
        size="sm"
        label="Create"
        :loading="false"
        :disabled="false"
        @click="createDialogShown = true"
      >
      </Button>
    </div>

    <!-- Dialog for Adding New Short Link -->
    <Dialog
       
      :options="{
        title: 'New Short Link',
        size: '2xl',
        actions: [
          {
            label: 'Save',
            variant: 'solid',
            onClick(close) {
              console.log('creating new ...')
              links.insert.submit({
                ...newLink
              }, {
                onSuccess(){
                  close();
                }
              })

              close();
            }
          }
        ]
      }" v-model="createDialogShown"
    >
      <template #body-content>
        <form calss ="space-y-3">
          <div class="p-2">
            <FormControl
              type="text"
              placeholder="Enter short link"
              label="Short Link"
              v-model="newLink.short_link"
            />
          </div>
          <div class="p-2">
            <FormControl
              type="text"
              placeholder="Enter destination URL"
              label="Destination URL"
              v-model="newLink.destination_url"
            />
          </div>
          <div class="p-2">
            <FormControl
              type="textarea"
              placeholder="Enter destination"
              label="Destination"
              v-model="newLink.description"
            />
          </div>
        </form>
      </template>

      <template #actions>

      </template>
    </Dialog>


    <!-- ListView Component -->
    <ListView
      v-if="links.list.data"
      :columns="[
        { label: 'Short Link', key: 'short_link', width: 0.4 },
        { label: 'Destination', key: 'destination_url' },
        { label: 'Description', key: 'description' }
      ]"
      :rows="links.list.data"
      :options="{
        showTooltip: false
      }"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { createListResource } from 'frappe-ui'
import { ListView, Dialog, FormControl} from 'frappe-ui'
import { reactive } from 'vue'


const createDialogShown = ref(false)

const newLink = reactive({
  short_link: '',
  destination_url: '',
  description: ''
})

const links = createListResource({
  doctype: "S Link",
  fields: ['short_link', 'destination_url', 'description'],
  orderBy: 'creation desc'
})

links.list.fetch()

</script>

