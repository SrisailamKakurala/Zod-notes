# 🧾 Zod Notes — Beginner to Advanced (Full Guide)

## 📦 What is Zod?

Zod is a **TypeScript-first schema declaration and validation library**. It provides runtime type checking for JavaScript objects, which TypeScript alone cannot do.

---

## 📌 Why Zod?

* ✅ Runtime validation (TS is only compile-time)
* ✅ Auto-infers TypeScript types from schemas
* ✅ Small and tree-shakable
* ✅ Works great with form libraries and API validations

---

## 🟢 Level 1: Basics

### 1.1 Primitive Types

```ts
z.string()       // string
z.number()       // number
z.boolean()      // boolean
z.bigint()       // bigint
z.date()         // Date object
z.undefined()    // undefined
z.null()         // null
z.literal("yes") // must be exactly "yes"
```

### 1.2 Parsing and Safe Parsing

```ts
const result = z.string().safeParse("Sri")
if (result.success) console.log(result.data)
```

### 1.3 Inferring Types

```ts
const UserSchema = z.object({ name: z.string() })
type User = z.infer<typeof UserSchema>
```

---

## 🟡 Level 2: Objects, Arrays, and Tuples

### 2.1 Object Schema

```ts
const userSchema = z.object({
  name: z.string(),
  age: z.number(),
})
```

### 2.2 Arrays

```ts
z.array(z.string()) // string[]
```

### 2.3 Tuples

```ts
z.tuple([z.string(), z.number()]) // [string, number]
```

### 2.4 Nested Objects

```ts
const nested = z.object({
  user: z.object({
    id: z.number(),
    info: z.object({ email: z.string().email() })
  })
})
```

---

## 🟠 Level 3: Unions, Enums, and Literals

### 3.1 Union

```ts
z.union([z.string(), z.number()])
```

### 3.2 Enums

```ts
const Color = z.enum(["Red", "Green", "Blue"])
type Color = z.infer<typeof Color>
```

### 3.3 Literals

```ts
z.literal("active")
```

### 3.4 Discriminated Unions

```ts
const animal = z.discriminatedUnion("type", [
  z.object({ type: z.literal("dog"), barkVolume: z.number() }),
  z.object({ type: z.literal("cat"), livesLeft: z.number() })
])
```

---

## 🔵 Level 4: Advanced Features

### 4.1 Merging Schemas

```ts
const A = z.object({ a: z.string() })
const B = z.object({ b: z.number() })
const C = A.merge(B) // { a: string, b: number }
```

### 4.2 Transform

```ts
z.string().transform((val) => val.toUpperCase())
```

### 4.3 Refinement

```ts
z.string().refine(val => val.length > 5, {
  message: "Too short"
})
```

### 4.4 Preprocess

```ts
z.preprocess((arg) => parseInt(arg as string), z.number())
```

---

## 🟣 Level 5: Async, Effects, and Errors

### 5.1 Async Parsing

```ts
await schema.parseAsync(data)
```

### 5.2 Custom Error Map

```ts
z.setErrorMap((issue, ctx) => {
  return { message: `Custom: ${issue.code}` }
})
```

---

## 🔴 Bonus: Utilities

### 6.1 Nullable & Optional

```ts
z.string().nullable()     // string | null
z.string().optional()     // string | undefined
```

### 6.2 Default Values

```ts
z.string().default("Sri")
```

### 6.3 Catchall for unknown keys

```ts
z.object({}).catchall(z.string())
```

### 6.4 Strict vs Passthrough

```ts
z.object({ name: z.string() }).strict()      // No extra keys allowed
z.object({ name: z.string() }).passthrough() // Extra keys allowed
```

---

## ✅ Real-World Use Case Example

### Form Validation in a Next.js Server Action

```ts
const FormSchema = z.object({
  email: z.string().email(),
  password: z.string().min(6)
})

export async function loginAction(formData: FormData) {
  "use server"
  const values = Object.fromEntries(formData)
  const parsed = FormSchema.safeParse(values)

  if (!parsed.success) return { error: parsed.error.format() }
  // Do login logic
}
```

---

## 🏁 Wrap-up

Zod is a powerful way to ensure **data safety at runtime** while keeping the benefits of TypeScript. It's especially useful when:

* Consuming API responses
* Handling form data
* Working with third-party input

Let me know if you want code snippets, exercises, or form integration examples! 💪
