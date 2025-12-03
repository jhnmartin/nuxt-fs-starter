<script setup lang="ts">
import * as z from 'zod';
import type { FormSubmitEvent } from '@nuxt/ui';

definePageMeta({
  layout: 'auth'
});

useSeoMeta({
  title: 'Sign up',
  description: 'Create an account to get started'
});

const supabase = useSupabaseClient();
const router = useRouter();
const toast = useToast();

const fields = [
  {
    name: 'first_name',
    type: 'text' as const,
    label: 'First Name',
    placeholder: 'Enter your first name'
  },
  {
    name: 'last_name',
    type: 'text' as const,
    label: 'Last Name',
    placeholder: 'Enter your last name'
  },
  {
    name: 'email',
    type: 'text' as const,
    label: 'Email',
    placeholder: 'Enter your email'
  },
  {
    name: 'password',
    label: 'Password',
    type: 'password' as const,
    placeholder: 'Enter your password'
  }
];

const providers = [
  {
    label: 'Google',
    icon: 'i-simple-icons-google',
    onClick: () => {
      toast.add({ title: 'Google', description: 'Login with Google' });
    }
  },
  {
    label: 'Apple',
    icon: 'i-simple-icons-apple',
    onClick: () => {
      toast.add({ title: 'Apple', description: 'Login with Apple' });
    }
  }
];

const schema = z.object({
  first_name: z.string().min(1, 'First name is required'),
  last_name: z.string().min(1, 'Last name is required'),
  email: z.email('Invalid email'),
  password: z.string().min(8, 'Must be at least 8 characters')
});

type Schema = z.output<typeof schema>;

async function onSubmit(payload: FormSubmitEvent<Schema>) {
  const { data, error } = await supabase.auth.signUp({
    email: payload.data.email,
    password: payload.data.password as string,
    options: {
      data: {
        first_name: payload.data.first_name,
        last_name: payload.data.last_name
      }
    }
  });

  if (error) {
    toast.add({ title: 'Error', description: error.message });
  }

  if (data.user) {
    toast.add({
      title: 'Success',
      description: 'Account created successfully'
    });
    router.push('/confirm');
  }
}
</script>

<template>
  <UAuthForm
    :fields="fields"
    :schema="schema"
    :providers="providers"
    title="Create an account"
    :submit="{ label: 'Create account' }"
    @submit="onSubmit"
  >
    <template #description>
      Already have an account?
      <ULink to="/login" class="text-primary font-medium">Login</ULink>.
    </template>

    <template #footer>
      By signing up, you agree to our
      <ULink to="/" class="text-primary font-medium">Terms of Service</ULink>.
    </template>
  </UAuthForm>
</template>
