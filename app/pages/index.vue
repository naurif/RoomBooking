<script setup lang="ts">
import type { AuthFormField, FormSubmitEvent } from '@nuxt/ui'
import * as z from 'zod'
definePageMeta ({
    layout:'auth'
})

const fields = ref<AuthFormField[]>([
  {
    name: 'username',
    type: 'text',
    label: 'Username'
  },
  {
    name: 'password',
    type: 'password',
    label: 'Password'
  }
])

const schema = z.object({
  username: z.string('Invalid Username').trim().min(3, "Username must be at least 3 characters"),
  password: z.string('Password is required').min(8, 'Must be at least 8 characters')
})

type Schema = z.output<typeof schema>

function onSubmit(payload: FormSubmitEvent<Schema>) {
  console.log('Submitted', payload)
    navigateTo({name:'book'})
}
</script>

<template>
    <div class="flex flex-col items-center justify-center gap-4 p-4">
        <UPageCard class="w-full max-w-md">
            <UColorModeButton class="w-8"/>
            <UAuthForm
                :schema="schema"
                title="Login"
                description="Enter your credentials to Book a Meeting Room."
                icon="i-lucide-user"
                :fields="fields"
                class=""
                @submit="onSubmit"
            />
        </UPageCard>
    </div>
</template>