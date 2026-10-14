# Padrão de Formulários e Prevenção de Concorrência

Convenções rigorosas para criação de formulários com React Hook Form, validação via Zod e controle de concorrência em mutações.

---

## 1. Estrutura Padrão de Formulário

Cada formulário complexo deve conter seus contratos isolados:

- **`schema.ts`**: Esquema de validação Zod.
- **`types.ts`**: Tipos estáticos derivados do esquema (`z.infer<typeof schema>`).
- **`index.tsx`**: Componente do formulário.

### Exemplo de `schema.ts`:
```typescript
import { z } from 'zod';

export const customerFormSchema = z.object({
  fullName: z.string().min(3, 'O nome deve ter pelo menos 3 caracteres'),
  email: z.string().email('E-mail inválido'),
  phone: z.string().min(9, 'Telefone obrigatório'),
  notes: z.string().optional(),
});

export type CustomerFormData = z.infer<typeof customerFormSchema>;
```

---

## 2. Padrão Anti-Concorrência (`useMutation` + `<fieldset disabled>`)

Para evitar cliques duplos, envios concorrentes e inconsistências visuais durante chamadas assíncronas ao servidor, todo formulário deve implementar esta tríade de proteção:

1. **Guarda no Hook (`useMutation`):**
   - Rejeita chamadas paralelas caso uma mutação já esteja em processamento (`isMutating === true`).
2. **Desativação Nativa em Massa (`<fieldset disabled={isMutating}>`):**
   - Envolva todo o conteúdo do `<form>` dentro de uma tag `<fieldset disabled={isMutating}>`.
   - Isso desativa instantaneamente todos os inputs, selects, textareas e botões daquele formulário de forma declarativa e nativa do navegador, sem necessidade de passar `disabled={loading}` manualmente para cada input.
3. **Botão de Submissão com Estado de Loading:**
   - O botão principal de envio recebe `loading={isMutating}` e exibe um spinner animado.

### Exemplo Completo no Componente:

```tsx
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { customerFormSchema, CustomerFormData } from './schema';
import { useMutation } from '@/hooks/use-mutation';
import { Button } from '@/components/ui/button';
import { InputFormControl } from '@/components/shared/form-control/input/input-form-control';

export function CustomerForm({ onSuccess }: { onSuccess?: () => void }) {
  const form = useForm<CustomerFormData>({
    resolver: zodResolver(customerFormSchema),
    defaultValues: { fullName: '', email: '', phone: '', notes: '' },
  });

  const { isMutating, execute } = useMutation();

  const onSubmit = (data: CustomerFormData) => {
    execute(async () => {
      const response = await createCustomerAction(data);
      if (response.success) {
        onSuccess?.();
      }
    });
  };

  return (
    <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
      <fieldset disabled={isMutating} className="space-y-4 contents">
        <InputFormControl
          control={form.control}
          name="fullName"
          label="Nome Completo"
          placeholder="Ex: João Silva"
        />

        <InputFormControl
          control={form.control}
          name="email"
          label="E-mail"
          placeholder="joao@empresa.com"
        />

        <Button type="submit" loading={isMutating} className="w-full">
          Salvar Cliente
        </Button>
      </fieldset>
    </form>
  );
}
```

---

## 3. Form-Controls Padronizados

- Não recrie inputs do zero com classes Tailwind espalhadas.
- Utilize componentes form-control reutilizáveis (`InputFormControl`, `TextareaFormControl`, `SelectFormControl`) que encapsulam:
  - `<FormItem>`
  - `<FormLabel>`
  - `<FormControl>` com estilos padrão (`bg-surface-secondary`, borda discreta, foco com ring azul)
  - `<FormMessage>` com feedback de erro inline do Zod.
